Hoisting: 
This is a behavior of the JavaScript engine during the Creation Phase. It scans the code for variable and function declarations and allocates memory for them before executing a single line of code.

Simpler Def : Js moves the variable and function declarations to the top before executing a single.

--------------------------------------------------------------------------------------------------

The Temporal Dead Zone (TDZ): 
While let and const are hoisted, the engine marks them as "uninitialized." The TDZ is the period between the start of the block and the moment the variable is declared. Accessing them in this "zone" triggers a ReferenceError

--------------------------------------------------------------------------------------------------
Closures
Def_1:Closure is simply a function that remembers its lexical scope even when its executed outside that scope.
--------------------------------------------------------------------------------------------------
To build the mental model for how the JavaScript engine treats your code before executing it, think of the JS Engine as a Theater Director staging a play 🎭. Before anyone speaks a line, the director walks around the stage, notes where every actor (variable/function) is supposed to stand, and sets up the props (memory allocation). Only after this setup phase does the actual performance (execution) begin.
---------------------------------------------------------------------------------------------------
#### 📦 Scope-Specific Hoisting
Hoisting is completely bound to the **current lexical scope** where the declaration lives.
When the engine runs the compilation phase for a scope, it only pulls declarations to the top of *that specific scope* (whether it is global scope, function scope, or a block `{}` scope). It never leaks across boundaries.

#### 🛑 Variable Collisions (Same vs. Different Scopes)
Interviewers watch closely to see if you understand how the engine's memory mapping behaves when names collide.
#### 1\. Same Scope Collision
If you try to declare two variables with the exact same name in the **same scope**:
-   `var`: The engine quietly allows it. The second declaration just acts as a reassignment or a harmless duplicate to the compiler.
-   `let` / `const`: The engine immediately throws a `SyntaxError: Identifier 'x' has already been declared`. The engine refuses to allocate a duplicate key in the same Environment Record.
#### 2\. Different Scope Collision (Shadowing)
If they are in **different scopes** (e.g., one global, one inside a function or a `{}` block):
-   This is called **Variable Shadowing**.
-   The engine allocates two completely separate spaces in memory. The inner variable "shadows" or hides the outer one. When executing code inside the inner scope, the engine looks at its local Environment Record first, finds the local variable, and stops looking further up the scope chain.
------------------------------------------------------------------------------------------------
#### The TDZ (Temporal Dead Zone) Nuance
> 💡 **The Senior Distinction:** Both `var` and `let`/`const` are hoisted. The difference is their **initialization**.
> -   `var` is hoisted and immediately initialized to `undefined`.
> -   `let`/`const` are hoisted but remain **uninitialized**.
The Temporal Dead Zone is not a specific place in memory; it is a **time window**. It is the time between entering a scope and the line of code where the `let`/`const` is actually initialized with a value. If you touch an uninitialized variable during this window, the engine throws a `ReferenceError`.
-------------------------------------------------------------------------------------------------
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}//output is 3 3 3
```
#### The Analogy: The Shared Notepad vs. Individual Post-its
-   **`var` (The Shared Notepad):** `var` is function-scoped (or global here). The engine creates **one single variable** `i` on a shared notepad. The loop runs to completion instantly, scratching out the old number and writing `3`. A second later, all three `setTimeout` callbacks look at that *same single notepad* and read `3`.

-   **`let` (Individual Post-its):** `let` is block-scoped. Every single iteration of the loop creates a **brand new block scope** (a new individual Post-it note). The engine copies the current value of `i` onto that specific Post-it. When the callbacks fire, each looks at its own unique Post-it note.
using let prints 1 2 3
```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```
convert the above function to execute with its own i val per iteration by keeping var and using IIFE
```js
for (var i = 0; i < 3; i++) {
  (function(currentI) {
    setTimeout(() => {
      console.log(currentI);
    }, 1000);
  })(i);
}
```
#### The Analogy Update
By wrapping `setTimeout` inside an IIFE, you are building a **private photo booth** 📸 around each iteration.
1. The loop runs and `i` changes on the global stage.
2. For each iteration, the IIFE snaps a photo of `i` at that exact moment and names it `currentI`.
3. The `setTimeout` callback looks at the photo (`currentI`), not the live variable on the stage.

### 🛠️ Is the IIFE the *only* way to create this snapshot?

Absolutely not! In an interview, if you show alternative ways to solve the same problem, it demonstrates a deep understanding of the language. Here are the **three major alternative patterns** to create a dedicated snapshot:

#### Pattern 1: The Named Helper Function (Cleaner Alternative to IIFE)

Instead of an anonymous function executed inline, you can break it out. This does the exact same thing under the hood---creating a new function scope per iteration.

JavaScript

```
function createSnapshot(currentI) {
  setTimeout(() => {
    console.log(currentI);
  }, 1000);
}

for (var i = 0; i < 3; i++) {
  createSnapshot(i); // Passing 'i' copies its value into the function's scope
}

```

#### Pattern 2: Harnessing the Native `setTimeout` Binding (The Elegant Way)

Many frontend developers forget that `setTimeout` accepts additional parameters! Any arguments passed to `setTimeout` after the delay millisecond argument are automatically passed as parameters to the callback function.

JavaScript

```
for (var i = 0; i < 3; i++) {
  // Pass 'i' as the third argument.
  // setTimeout will cache its value and pass it to the callback when it executes.
  setTimeout((snapshotOfI) => {
    console.log(snapshotOfI);
  }, 1000, i);
}

```

#### Pattern 3: Using `.bind()` (The ES5 Classic)

You can explicitly create a new function instance where the argument is pre-bound (locked in place).

JavaScript

```
for (var i = 0; i < 3; i++) {
  setTimeout(
    function(snapshotOfI) {
      console.log(snapshotOfI);
    }.bind(null, i), // Pre-binds 'i' as the first argument to this function instance
    1000
  );
}

```
---

#### React Real-World Connection: Stale Closures

This exact mechanism is why developers run into **stale closures** in React. If you capture a state variable inside a `useEffect` with an asynchronous operation, it locks into that specific render's value.
Let's look at a classic senior interview scenario involving React state.

```
function Counter() {
  const [count, setCount] = useState(0);

  const handleAlertClick = () => {
    setTimeout(() => {
      alert('You clicked on: ' + count);
    }, 3000);
  };

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  );
}

```

**The Scenario:**

1. The user clicks "Show alert" when the count is `0`.
2. The user quickly clicks "Click me" three times, so the UI reads "You clicked 3 times".
3. The 3-second timeout finishes.

### 🎭 The Analogy: The Flipbook Animation 📖

Think of every render in React as a single page in a flipbook.

-   **Render 1 (Count = 0):** The page is drawn. On this page, `count` is a constant `0`. The `handleAlertClick` function on this specific page looks at `count` and sees `0`. When you click "Show alert", you schedule a timer from *this specific page*.

-   **Render 2, 3, 4 (Count = 1, 2, 3):** Clicking "Click me" forces React to rip out the old page and draw a brand new page each time. On the latest page, `count` is `3`.

The timeout you triggered belongs to the **first page**. It trapped the value `0` in its closure.

### Level Up: The Interviewer's Pivot

An interviewer will follow up with: *"Excellent. Now, what if the product requirement changes? The product manager says: 'When the alert fires after 3 seconds, it **must** show the absolute latest, most up-to-date count, even if the user clicked the button when it was 0.' How do you fix this without causing extra re-renders?"*

A `useRef` object is like a **locked storage locker** 🗄️ that sits *outside* the React flipbook. Every time React re-renders a new page, the storage locker stays exactly where it is in memory. The reference to the locker (`ref.current`) never changes, but the *contents* inside it can be updated at any time without triggering a re-render.

### 🛠️ The Solution Pattern: The "Latest Ref" Pattern

This is a premier senior-level pattern used heavily in production libraries (like React Use or Formik) to solve stale closure problems.

JavaScript

```
function Counter() {
  const [count, setCount] = useState(0);

  // 1. Create the locker
  const countRef = useRef(count);

  // 2. Keep the locker content updated on every render
  countRef.current = count;

  const handleAlertClick = () => {
    setTimeout(() => {
      // 3. Read directly from the live locker, not the stale closure
      alert('You clicked on: ' + countRef.current);
    }, 3000);
  };

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  );
}

```

### 🔍 Why This works (The Interviewer's Check)

1.  **No Re-renders:** Modifying `countRef.current = count` does *not* trigger a re-render because mutating a ref object doesn't change component state.

2.  **Always Fresh:** When the `setTimeout` fires 3 seconds later, the closure doesn't look at the historical `count` primitive variable. Instead, it looks at `countRef`. Because `countRef` points to the *same stable object instance* across all renders, reading `.current` pulls the absolute freshest value.

### 🧱 Summary of Key Patterns

Before we jump to the test questions, let's solidify the architectural rules you've mastered today:

-   **Hoisting Separation:** `var` is hoisted and initialized (`undefined`). `let`/`const` are hoisted but uninitialized, creating a Temporal Dead Zone (TDZ) time-window where access throws a `ReferenceError`.

-   **Scope Isolation:** `let` inside a loop header allocates a brand-new scope environment record per iteration. `var` creates a single variable across the entire outer function/global scope.

-   **React Closures:** Functions defined inside a React component capture the state variables *at the moment of that specific render cycle*.

-   **The Ref Escape Hatch:** To read live, mutating values inside an older asynchronous closure without causing re-renders, pipe the state into a mutable `useRef`.

#### Question 1: Function vs. Variable Hoisting

What is logged, and why?

JavaScript

```
var foo = 1;
function foo() {
  return 2;
}
console.log(typeof foo);
```

#### 🎭 The Analogy: The Land Grab 🗺️

Think of the compilation phase as a land grab. **Functions are VIPs**---they get hoisted and assigned memory first. Variables (`var`) get hoisted second. However, if a variable has the *same name* as a function, the engine sees the variable declaration as a duplicate and ignores it... **UNTIL execution starts**.

Let's look at how the engine rewrites your code under the hood during execution:

JavaScript

```
// 1. Compilation Phase (Functions first, then vars)
function foo() { return 2; }
// var foo; // Ignored by the engine because 'foo' already exists as a function

// 2. Execution Phase (Line by line)
foo = 1; // Overwrites the function with the number 1!

console.log(typeof foo); // 'number'

```

-   **The Interviewer Takeaway:** Functions are hoisted before variables. However, a variable *assignment* (`foo = 1`) happens during execution and will completely overwrite the hoisted function reference.

#### Question 2: The Double TDZ Trap

What happens when `bar()` is executed?

JavaScript

```
let x = 10;
function bar() {
  console.log(x);
  let x = 20;
}
bar();

```

#### Question 3: The React Async Batching Machine

If a user clicks this button *once*, what gets logged to the console, and what is the final state of `count`?

JavaScript

```
function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    console.log('Log 1:', count);

    setCount(prev => prev + 1);
    console.log('Log 2:', count);
  };

  return <button onClick={handleClick}>Run</button>;
}
```

### ⏳ Question 2: The Double TDZ Trap

> **Your Answer:** `ReferenceError` because of the inner TDZ.
>
> **Actual Output:** Correct! `ReferenceError: Cannot access 'x' before initialization`.

Your reasoning is flawless. The inner `let x` creates a block scope that shadows the outer `x`. Because it is `let`, it creates a Temporal Dead Zone from the moment the function starts until the line `let x = 20`. Trying to log it in that window crashes the program.

### ⚛️ Question 3: The React Async Batching Machine

> **Your Answer:** Log 1: `0`, Log 2: `0`, Final State: `1`.
>
> **Actual Output:** Log 1: `0`, Log 2: `0`, Final State: `2`.

You correctly identified that both logs print `0` because of batching and the snapshot nature of state within a single render cycle. However, you fell into the trap regarding the **functional updater** (`prev => prev + 1`).

#### 🎭 The Analogy: The Waiter's Order Pad 📝

When you call `setCount`, you aren't changing the state instantly. You are handing a note to a waiter (React) with instructions.

1.  `setCount(count + 1)` $\\rightarrow$ You tell React: *"Set the next state to 0 + 1 = 1."* React puts this on its notepad.

2.  `setCount(prev => prev + 1)` $\\rightarrow$ You hand React a recipe: *"Take whatever the pending state is at the end of the queue, and add 1 to it."*

When the event handler finishes, React processes the notepad queue:

-   First instruction: Set state to `1`. (Pending state is now `1`).

-   Second instruction: Run the recipe `prev => prev + 1` using the pending state. $1 + 1 = 2$.

Therefore, the final state becomes **`2`**.

-   **The Interviewer Takeaway:** Passing a primitive value to `setCount` replaces the state based on the *current render's snapshot*. Passing a functional updater (`prev => ...`) queues a calculation that executes against the *latest pending state* in the batch queue.


### 💻 Deep-Dive Scenario: The Race Condition

Look at this very common real-world senior interview scenario:

JavaScript

```
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    let isCurrentRequest = true;

    fetch(`https://api.example.com/user/${userId}`)
      .then(res => res.json())
      .then(data => {
        if (isCurrentRequest) {
          setUser(data);
        }
      });

    return () => {
      isCurrentRequest = false;
    };
  }, [userId]);

  return <div>{user ? user.name : 'Loading...'}</div>;
}

```

**The Question:** How does the `isCurrentRequest` boolean variable use JavaScript closures to prevent a **race condition** if a user rapidly switches between `userId = 1` and `userId = 2`? Walk me through what happens to the variables in memory.

Your analysis of how `useEffect` triggers is highly accurate, but your understanding of what happens when the network requests resolve is exactly where the "senior-level trap" lies.

Let's look at why your assumption---*"whichever fetch returns first, that `setUser` will run first"*---creates a major bug, and how closures elegantly fix it.

### 🚨 The Blind Spot: The Slow Network Race Condition

You are assuming that the API requests will return in the exact order they were sent. In the real world, networks are completely unpredictable.

Imagine this timeline **without the flag**:

1.  Component mounts with `userId = 1`. `useEffect` fires. **Fetch #1** is launched.

2.  The user immediately clicks a button changing `userId = 2`. The component re-renders.

3.  `useEffect` fires again because `userId` changed. **Fetch #2** is launched.

4.  **Fetch #2 resolves incredibly fast** (takes 100ms). It calls `setUser(Data_For_User_2)`. The UI correctly shows User 2.

5.  **Fetch #1 finally resolves** (it was stuck in network traffic for 2000ms). It calls `setUser(Data_For_User_1)`.

**The Bug:** The UI now displays data for User 1, even though the active `userId` prop is `2`! The data on the screen is completely out of sync with the application state.

### 🎭 The Analogy: The Secure Mailing Tube 🧪

Think of each `useEffect` execution as spawning a completely isolated, sealed mailing tube in memory.

-   **Tube 1 (Render 1, `userId = 1`):** Has its own private variable `isCurrentRequest = true`.

-   **Tube 2 (Render 2, `userId = 2`):** Has its own *completely separate* private variable `isCurrentRequest = true`.

### 🕵️‍♂️ Step-by-Step: How Closures Fix the Race

Now, let's trace the exact execution with the flag and the cleanup function when the user switches rapidly from `userId = 1` to `userId = 2`:

1.  **`userId = 1` (Tube 1 opens):** \* `isCurrentRequest` is initialized to `true` inside Tube 1's scope.

        -   **Fetch #1** is launched. The `.then()` callback is a closure---it wraps around Tube 1's variables and holds a reference to *this specific* `isCurrentRequest`.

2.  **The Switch (`userId` changes to 2):**

        -   Before React runs the effect for `userId = 2`, it **must execute the cleanup function of the previous effect**.

        -   React invokes the cleanup function from **Tube 1**.

        -   Because of closures, that cleanup function reaches back into Tube 1's memory space and flips *Tube 1's* `isCurrentRequest` to `false`.

3.  **`userId = 2` (Tube 2 opens):**

        -   A completely brand new scope is created.

        -   `isCurrentRequest` inside Tube 2 is initialized to `true`.

        -   **Fetch #2** is launched. Its `.then()` callback closes over Tube 2's `isCurrentRequest` (which is currently `true`).

4.  **The Network Resolves:**

        -   **Fetch #2 completes first.** It enters its callback, checks Tube 2's `isCurrentRequest`. It is `true`. `setUser(Data_2)` runs. UI shows User 2.

        -   **Fetch #1 completes last.** It enters its callback, reaches back into Tube 1's scope via its closure, and checks Tube 1's `isCurrentRequest`. **It reads `false`** (because the cleanup flipped it!).

        -   The `if (isCurrentRequest)` block evaluates to `false`. **`setUser(Data_1)` is completely ignored.**

### 🎯 The Senior Takeaway

Interviewers love this pattern because it proves you understand that **cleanup functions execute in the context of the render they were created in**, leveraging closures to securely target and neutralize stale asynchronous callbacks floating in the event loop.

It is explicitly because `isCurrentRequest` is **referenced inside** the `.then` callback.

To ace this in a senior interview, you need to draw a sharp line between **what a closure *could* capture** versus **what the JavaScript engine *actually* keeps in memory**.

### 🎭 The Analogy: The Moving Box 📦

Think of a function's scope like a house you are moving out of. When the function finishes executing (`useEffect` finishes running its main body), the engine wants to tear the house down to free up memory (Garbage Collection).

-   **If you don't reference anything:** The engine throws everything away.

-   **If you reference a variable inside `.then`:** It's like putting that specific item into a labeled moving box (`Closure`) and taking it with you. The engine cannot tear down that specific item because the `.then` callback still holds a key to it.

### ⚙️ How the Engine Handles This Under the Hood

JavaScript uses a mechanism called **Lexical Scoping**. A closure is created at the moment a function is *defined*, not when it is executed.

When you write this line:

JavaScript

```
.then(data => {
  if (isCurrentRequest) { // <-- The engine sees this!
    setUser(data);
  }
});

```

The JS compiler analyzes the code and says: *"Aha! This anonymous callback function is using `isCurrentRequest` and `setUser`, which belong to the outer `useEffect` scope. I must keep a reference to these specific variables alive in a **Closure Object** attached to this callback function."*

#### What if a variable isn't referenced?

If you declared another variable inside the effect, like `const debugName = "FetchUser";`, but you **never** used it inside the `.then` callback or the cleanup function, the engine is smart. It optimizes memory and **garbage collects** `debugName` as soon as the `useEffect` function finishes executing. It will *not* be part of the closure.

### 1\. Modeling State with the Minimal Shape

**The Core Concept:** You should never store a value in state if it can be computed synchronously from existing state or props during the render cycle.

#### 🎭 The Analogy: The Restaurant Menu 📋

Imagine a restaurant menu that lists the price of a burger ($10), the tax ($1), and the *total price* ($11) explicitly printed on the page. If the price of the burger changes to $12, the waiter now has to manually change *both* the price and the total price. If they forget to update the total, the menu becomes buggy and inconsistent.

Instead, the menu should only store the base price ($12) and the tax rate (10%). The total price should be **computed on the fly** when the customer looks at it.

#### 💻 The Interview Problem

An interviewer shows you this code and asks you to optimize it for a senior level:

JavaScript

```
// ❌ Poor Pattern: Storing derived values in state
function Cart({ items }) {
  const [totalPrice, setTotalPrice] = useState(0);
  const [itemCount, setItemCount] = useState(0);

  useEffect(() => {
    setTotalPrice(items.reduce((sum, item) => sum + item.price, 0));
    setItemCount(items.length);
  }, [items]);

  return <div>Total ({itemCount} items): ${totalPrice}</div>;
}

```

#### 🛠️ The Senior Fix

Get rid of `useEffect` and extra states entirely. Compute them inline during the render phase.

JavaScript

```
//  Senior Pattern: Compute on the fly
function Cart({ items }) {
  // Synchronous calculations happen instantly on every render cycle
  const itemCount = items.length;
  const totalPrice = items.reduce((sum, item) => sum + item.price, 0);

  return <div>Total ({itemCount} items): ${totalPrice}</div>;
}

```

-   **Why this rules:** It eliminates unnecessary re-renders. The ❌ version caused **two** render cycles every time `items` changed (one for the prop change, and a second one when `useEffect` updated the state). The senior version computes it in **one single render pass** and guarantees the data can never fall out of sync.

### 2\. Lifting State Up vs. Keeping it Local

**The Framework for Decision Making:** In a senior interview, don't just say "lift it up if two components need it." Frame it around **Component Cohesion** and **Re-render Scope**.

| **Strategy** | **When to Use** | **Architectural Reason** |
| --- |  --- |  --- |
| **Keep Local** | Form inputs, dropdown toggle states, hover states, tabs inside a single widget. | **Encapsulation & Performance:** Prevents parent components and sibling components from re-rendering unnecessarily when typing or toggling. |
| **Lift State Up** | Two components need to mutate or display the exact same changing data (e.g., a search bar filtering a list view). | **Single Source of Truth:** Coordinates state sync between distant branches of the UI tree. |

### 3\. The Pitfalls of Duplicate State

Duplicate state happens when you copy a prop into local state, or mirror one state value into another.

#### 💻 The Interview Problem (The Stale Sync Bug)

JavaScript

```
// ❌ Poor Pattern: Mirroring props into state
function ProductDetails({ product }) {
  const [price, setPrice] = useState(product.price);

  return <div>Price: ${price}</div>;
}

```

#### 🚨 The Structural Breakdowns (What goes wrong)

1.  **The Stale Prop Bug:** If the parent component changes the `product` prop (e.g., the user navigates to a different product, or a WebSocket updates the database), the `ProductDetails` component **will not update**. Why? Because `useState` only initializes *once* when the component mounts. Subsequent renders completely ignore the initial value passed to `useState`.

2.  **Split Source of Truth:** You now have two sources of truth for the price: the parent's data array and the child's local state. If they diverge, debugging becomes a nightmare.

#### 🛠️ The Senior Mitigation

If you *absolutely must* create a local copy of a prop because the user needs to edit it locally before saving, use an explicit React `key` to reset the component when the data identity shifts:

JavaScript

```
// Resetting state automatically via identity mapping
<ProductDetails product={activeProduct} key={activeProduct.id} />

```

By changing the `key`, you tell React to throw away the old fiber node instance, wipe its stale local state, and recreate a clean instance using the new prop value.

> ⚠️ *"Context solves the prop-drilling problem, but it creates a massive performance problem: every single component that consumes that context will re-render whenever *any* property on the context value object changes. How do you optimize this to prevent massive, unnecessary re-renders across the app?"*

Before we head to Day 2, let's look at the two premier patterns senior engineers use to keep Context lightning-fast.

### 1\. Splitting the State and Updater Contexts

Instead of putting both your state and your modification functions into one single context object, you split them into two separate pipelines.

#### 🎭 The Analogy: The PA System vs. The Suggestion Box 📣📬

-   **State Context (The PA System):** Only broadcasts the actual data. Components that need to *display* the numbers listen here.

-   **Dispatch Context (The Suggestion Box):** A one-way drop-off slot where components can drop off action instructions (like clicking a button). This box *never* changes its identity, so components that only need to trigger actions never re-render when the state updates.

#### 💻 The Senior Code Pattern

JavaScript

```
const StateContext = createContext(null);
const DispatchContext = createContext(null);

export function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, initialState);

  return (
    <StateContext.Provider value={state}>
      {/* dispatch never changes its reference identity across renders */}
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </StateContext.Provider>
  );
}

```

-   **Why this rules:** A component like `Grandchild` that only needs to trigger a click can consume `DispatchContext`. When the state changes, `Grandchild` **will not re-render** because the dispatch function reference remains completely stable.

### 2\. Using `useMemo` at the Provider Level

If you must use a single context, you must ensure the value object doesn't get a new reference identity on every single render of the parent provider.

JavaScript

```
export function UserProvider({ userId, children }) {
  const [theme, setTheme] = useState('dark');

  // ❌ BAD: This object literal gets a brand new memory reference on EVERY render
  // const contextValue = { theme, setTheme };

  //  SENIOR FIX: Memoize the object reference
  const contextValue = useMemo(() => ({
    theme,
    setTheme
  }), [theme]); // Only changes reference if theme changes

  return (
    <UserProvider.Provider value={contextValue}>
      {children}
    </UserProvider.Provider>
  );
}

```
