```js
//----------------------------------------------

//3. Write `once(fn)` decorator.
//This func ensures a piece of code or func to execute only once and then return the cached output the next time 
//This is similar to usememo hook in react

function threeOnce(fn){
    let firstTime= true;
    let result;
    return function(...args){
        if(firstTime){
            firstTime = false;
            result = fn(...args);
          }
        return result;
    }
}

/*const threeOnceTest = threeOnce(function (){
    console.log("First time calling");
    return 99;
});*/
//console.log(threeOnceTest());
//console.log(threeOnceTest());

//---------------------------------------------

//4. Write `curry(fn)` for fixed arity.
//Currying is a functional programming technique that transforms a function 
//with multiple arguments into a sequence of nesting functions, 
//each taking a single argument. 
//Instead of taking all inputs at once, a curried function accepts the first argument
//, returns a new function that accepts the second argument, and continues this
//pattern until all parameters are supplied.

function fourCurry (fn){
    const arity = fn.length;
    return function curriedFn(...args){
        if(args.length >= arity){
            return fn(...args)
        }
        else {
            return function (...moreArgs){
                return curriedFn( ...args,...moreArgs);
            }
        }
    }
}

//var addFour = fourCurry((a,b,c)=>a+b+c);
//addFour(2)(3)(4);

//------------------------------

//5. Write `debounce(fn, delay)`.
//every new call cancels the previous timer and starts a fresh one.
function fiveDebounce(fn,delay){
    let timerId;
    return function (...args){
        clearTimeout(timerId);
        timerId = setTimeout(() => {
            fn.apply(this,args)
        },delay)
    }
}
function fivesearch(val) {
    console.log("search fired with:", val);  // side effect so you can see it run
}
//const fiveHandleSearch = fiveDebounce(fivesearch, 300);
// Simulate rapid calls — only last one should fire
//fiveHandleSearch(10);   // scheduled, then cancelled
//fiveHandleSearch(20);   // scheduled, then cancelled  
//fiveHandleSearch(30);   // scheduled — this one survives

//------------------------------------

//6. Write `throttle(fn, delay)`.
function sixThrottle(fn,delay){
    let lastExecutedDate = 0;
    return function(...args){
        const curTime = Date.now();
        const curDelay = curTime - lastExecutedDate;
        if(curDelay >= delay){
            lastExecutedDate = curTime;
            return fn.apply(this,args);
        }
    }
}

//with cancel
function sixThrottle2(fn, delay) {
  let lastExecutedDate = 0;
  let timer = null;

  function throttled(...args) {
    const curTime = Date.now();
    const curDelay = curTime - lastExecutedDate;
    if (curDelay >= delay) {
      lastExecutedDate = curTime;
      return fn.apply(this, args);
    }
  }
  // Attach cancel directly onto the returned function
  throttled.cancel = function () {
    clearTimeout(timer);   // Kill any pending trailing call
    timer = null;
    lastExecutedDate = 0;  // Reset clock → next call fires immediately
  };

  return throttled;
}

//for trailing with last args inside function only
function throttleTrailing(fn, delay) {
  let timer = null;
    return function (...args) {
      latestArgs = args;        // always capture latest
      if (!timer) {
        timer = setTimeout(() => {
          fn.apply(this, latestArgs);  // use latest, not first
          timer = null;
        }, delay);
      }
    };
}

//-----------------------------------------

//7. Write `compose(...fns)`.

function sevencompose(...fns){
    return function(x){
        return fns.reduceRight((acc,fn) => fn(acc),x);
        
    }
}

//-----------------------------------------

//8. Write pipe(...fns)

function eightPipe(...fns) {
  return function (x) {
    return fns.reduce((acc, fn) => fn(acc), x);
  };
}

const trim       = str => str.trim();
const capitalize = str => str[0].toUpperCase() + str.slice(1);
const exclaim    = str => str + "!";

//const format = eightPipe(trim, capitalize, exclaim);
//console.log(format("  hello world  ")); // "Hello world!"

//---------------------------------------------

```

```js
//---------------------------------------------

//9. Write `pick(obj, keys)`.

function ninePick(obj,keys){
    let pickedObj={};
    for(const key of keys){
        if(key in obj){
           pickedObj[key] = obj[key] ;
        }
    }
    return pickedObj;
}

//option 2 using reduce
function ninePick2(obj,keys){
    return keys.reduce((acc,key) => {
        if(key in obj){
            acc[key] = obj[key]
        }
        return acc;
    },{})
}

//const obj = { a: undefined, b: 2 };
//"a" in obj          // true  ✅ — key EXISTS, value just happens to be undefined
//obj["a"] !== undefined  // false ❌ — wrongly concludes key doesn't exist

//--------------------------------------

//10. Write `omit(obj, keys)`.
function tenOmit(obj,keys){
    let omittedKeyObj = {};
    const distinctKeys = new Set(keys);
    for(const key in obj){
        if(obj.hasOwnProperty(key) && !distinctKeys.has(key)){
            omittedKeyObj[key] = obj[key]
        }
    }
    return omittedKeyObj
}

//option 2
function tenOmit2(obj,keys){
    const distinctKeys = new Set(keys); 
    return obj.reduce((acc,key)=>{
        if(!distinctKeys.has(key)){
            acc[key] = obj[key]
        }
        return acc;
    },{})
}



```
### Why `new Set(keys)` --- not just `keys.includes(key)`?

js
```
// Without Setkeys.includes(key)// O(n) --- scans the array every single iteration// With SetkeySet.has(key)// O(1) --- instant lookup
```

If `obj` has 1000 keys and `keys` has 100 items --- `includes` does 100,000 comparisons. `Set` does 1000. Small detail, big signal to an interviewer.

* * * *

### `for...in` vs `Object.keys()` --- why `hasOwnProperty` matters

js
```
for(const key in obj)// walks OWN + INHERITED prototype keys ⚠️Object.keys(obj)// walks OWN keys only ✅
```

If you use `for...in`, you must guard with `hasOwnProperty`. If you use `Object.keys`, it's already safe --- your choice in the `reduce` version is cleaner for that reason.

* * * *

### Pick vs Omit side by side

js
```
const obj ={a:1,b:2,c:3,d:4};pick(obj,["a","b"]);// → { a: 1, b: 2 }       iterate keys arrayomit(obj,["a","b"]);// → { c: 3, d: 4 }       iterate object
```


This is one of the most important topics in JavaScript --- and also the most commonly misunderstood even at senior level. Let me build it from the ground up, layer by layer.

* * * *

The one rule that explains everything
-------------------------------------

> `this` is not about where the function is *defined*. It is about how the function is *called*.

That single sentence resolves 90% of `this` confusion. Let's prove it across every scenario.

* * * *

Case 1 --- Global / plain function call
-------------------------------------

```
function sayHi() {
    console.log(this);
}

sayHi();

```

Nobody owns this call. No object before the dot.

```
browser:      this = window
strict mode:  this = undefined
Node.js:      this = global

```

* * * *

Case 2 --- Method call (object before the dot)
--------------------------------------------

```
const user = {
    name: "Vijay",
    greet() {
        console.log(this.name);
    }
};

user.greet();   // "Vijay" --- user is before the dot, this = user

```

The object *left of the dot at call time* becomes `this`. Not where `greet` was defined --- where it was called from.

```
// watch what happens when you detach it
const fn = user.greet;
fn();           // undefined --- no object before dot, this = window/undefined

```

This is the core gotcha. The function is identical. The call site changed. `this` changed.

* * * *

Case 3 --- Constructor call (new keyword)
---------------------------------------

```
function User(name) {
    this.name = name;         // this = the brand new object being created
}

const u = new User("Vijay");
console.log(u.name);          // "Vijay"

```

`new` creates a fresh object, sets `this` to it inside the function, then returns it.

* * * *

Case 4 --- Arrow functions (no `this` of their own)
-------------------------------------------------

```
const user = {
    name: "Vijay",
    greet() {
        const inner = () => {
            console.log(this.name);   // inherits this from greet's scope
        };
        inner();
    }
};

user.greet();   // "Vijay" --- arrow inherited this from greet, where this = user

```

Arrow functions don't get their own `this`. They look outward to whatever `this` was in the scope where they were *written*. This is called lexical `this`.

* * * *

Now let me show all four cases visually before going further.---

Why ES5 React needed `.bind(this)`
----------------------------------

In ES5 class-style React (before hooks), you wrote components like this:

```
class SearchBox extends React.Component {
    constructor(props) {
        super(props);
        this.state = { query: "" };
        this.handleChange = this.handleChange.bind(this); // ← why?
    }

    handleChange(e) {
        this.setState({ query: e.target.value }); // uses this
    }

    render() {
        return <input onChange={this.handleChange} />;
    }
}

```

What happens without `.bind(this)`:

```
// React internally does something like:
const handler = this.handleChange;  // detached from the instance
handler(event);                     // plain call --- this = undefined
                                    // this.setState → TypeError

```

When you pass `this.handleChange` to `onChange`, React stores the reference and calls it later --- without the object. The dot is gone. `this` is lost.

`.bind(this)` creates a *new function* that permanently locks `this` to the component instance, no matter how it's called later.

```
this.handleChange = this.handleChange.bind(this);
// now handleChange always has this = the component instance
// even when React calls it as a plain function

```

* * * *

`call`, `apply`, `bind` --- the complete picture
----------------------------------------------

```
const user = { name: "Vijay" };
const admin = { name: "Rahul" };

function greet(greeting, punctuation) {
    console.log(greeting + ", " + this.name + punctuation);
}

```

### `call` --- invoke immediately, args listed individually

```
greet.call(user, "Hello", "!");    // "Hello, Vijay!"
greet.call(admin, "Hi", ".");      // "Hi, Rahul."

```

Use when: you know the args at write time, you want immediate execution.

### `apply` --- invoke immediately, args as array

```
greet.apply(user, ["Hello", "!"]);   // "Hello, Vijay!"
greet.apply(admin, ["Hi", "."]);     // "Hi, Rahul."

```

Use when: args are already in an array at runtime (e.g. coming from `...args`, or from an API response).

```
// real case: finding max in an array (pre-ES6)
const scores = [44, 91, 23, 78];
Math.max.apply(null, scores);   // 91
// today: Math.max(...scores)

```

### `bind` --- returns a new function, doesn't call immediately

```
const greetVijay = greet.bind(user, "Hello");  // this locked, greeting pre-filled
greetVijay("!");    // "Hello, Vijay!"
greetVijay("?");    // "Hello, Vijay?"

```

Use when: you need to pass a function somewhere (event handler, callback, setTimeout) and want `this` locked ahead of time.

* * * *

Real-world scenarios for each
-----------------------------

### `call` --- method borrowing

```
// Array-like objects (e.g. arguments) don't have array methods
function logAll() {
    // arguments is array-like but not a real array
    const arr = Array.prototype.slice.call(arguments);
    console.log(arr);
}
logAll(1, 2, 3);   // [1, 2, 3]

```

### `apply` --- runtime argument arrays

```
// Logger that forwards to console with a prefix
function log(level, ...args) {
    console[level].apply(console, args);  // args unknown at write time
}
log("warn", "something", "went", "wrong");

```

### `bind` --- event handlers and callbacks

```
class Timer {
    constructor() {
        this.count = 0;
        // without bind, this inside tick = undefined when called by setInterval
        this.tick = this.tick.bind(this);
    }
    tick() {
        this.count++;
        console.log(this.count);
    }
    start() {
        setInterval(this.tick, 1000);
    }
}

```

### `bind` --- partial application (pre-filling args)

```
function multiply(a, b) {
    return a * b;
}

const double = multiply.bind(null, 2);  // a is locked to 2
double(5);   // 10
double(9);   // 18

```

`null` because `multiply` doesn't use `this` --- you only care about locking the first argument.

* * * *

The priority order (for senior interviews)
------------------------------------------

When multiple rules could apply, JS follows this precedence:

```
1. new keyword          → this = new object
2. call / apply / bind  → this = whatever you pass
3. method call (dot)    → this = object left of dot
4. plain call           → this = window / undefined

```

Arrow functions sit outside this ladder entirely --- they never enter the `this` determination process at all.

* * * *

Now you tell me --- take this scenario and tell me what `this` is at each marked point, and why:

```
const obj = {
    name: "Vijay",
    outer() {
        console.log(this.name);    // A --- what is this?

        const inner = function() {
            console.log(this);     // B --- what is this?
        };
        inner();

        const arrowInner = () => {
            console.log(this.name); // C --- what is this?
        };
        arrowInner();
    }
};

const detached = obj.outer;
detached();                        // D --- what does A print now?

obj.outer();                       // E --- normal call

```

Walk me through A, B, C, D, E --- what prints and why. That'll tell me exactly where your mental model is solid and where we need to sharpen it.

Good attempt --- you got C and E direction right, but A, B, D need sharpening. Let me tell you exactly where the gap is.

* * * *

Your answers vs reality
-----------------------

### A --- `obj.outer()` → `console.log(this.name)` inside `outer`

You said: `this` refers to `obj` ✅ --- correct.

`obj.outer()` --- object left of the dot is `obj`, so `this = obj`, prints `"Vijay"`.

* * * *

### B --- `inner()` → `console.log(this)`

You said: `this` refers to `inner` ❌

This is the most common misconception. `this` never refers to the function itself. Remember the rule --- **`this` is determined by how the function is called, not what it is.**

```
const inner = function() {
    console.log(this);
};
inner();   // plain call --- no object before the dot

```

No dot, no object, plain call → `this = undefined` (strict mode) or `window` (non-strict).

The fact that it's defined *inside* `outer` changes nothing. Definition location is irrelevant. Call site is everything.

* * * *

### C --- `arrowInner()` → `console.log(this.name)`

You said: `this` refers to `obj` ✅ --- correct, but I want to make sure you know *why*.

Arrow function has no own `this`. It looks outward to where it was *written* --- inside `outer`, where `this = obj`. So it inherits `obj`. Prints `"Vijay"`.

* * * *

### D --- `detached()` → what does A print now?

You said: `undefined` or error --- half right, but the reasoning matters.

```
const detached = obj.outer;
detached();

```

`detached()` is a plain call. No dot, no object. So inside `outer`, `this = undefined` (strict) or `window` (non-strict).

So at point A: `this.name` → `undefined.name` → **TypeError in strict mode**, or `window.name` which is `""` in a browser.

But here's what's interesting --- what happens to B and C now:

```
// B: inner() --- still a plain call
// this = undefined/window --- same as before, unchanged

// C: arrowInner --- looks outward to outer's this
// outer's this is now undefined (detached call)
// so arrowInner's this = undefined too
// this.name → TypeError

```

C changes based on how `outer` was called. That's the key insight about arrow functions --- they don't lock `this` to `obj` permanently. They lock it to *whatever `this` is in the outer scope at call time*.

* * * *

The corrected full picture
--------------------------

| Point | Call | `this` | Prints |
| --- |  --- |  --- |  --- |
| E→A | `obj.outer()` | `obj` | `"Vijay"` |
| --- |  --- |  --- |  --- |
| E→B | `inner()` plain | `undefined`/`window` | `undefined`/Window object |
| E→C | arrow, inherits outer | `obj` | `"Vijay"` |
| D→A | `detached()` plain | `undefined`/`window` | TypeError or `""` |
| D→C | arrow, inherits outer | `undefined`/`window` | TypeError or `""` |

* * * *

The one gap to fix in your mental model
---------------------------------------

You said `this` inside `inner` refers to `inner` --- this suggests you're thinking `this = the function`. That's never true.

A clean way to remember it:

> `this` is always an **object** (or undefined). Never a function. Never a variable name. Always ask --- "what object is *calling* this function right now?"

If no object is calling it, the answer is `window` or `undefined`. If an object is calling it via dot notation, that object is `this`.

* * * *

Ready for round two? Try this one --- `call`, `bind`, and arrow all together:

```
const user = {
    name: "Vijay",
    greet() {
        console.log(this.name);
    }
};

const admin = { name: "Rahul" };

const arrowGreet = () => {
    console.log(this.name);
};

user.greet();                      // 1
user.greet.call(admin);            // 2
const bound = user.greet.bind(admin);
bound();                           // 3
arrowGreet.call(user);             // 4 --- trick question

```

Tell me what each prints and why. Pay special attention to 4.

1, 2, 3 are perfect. 4 is where the gap is --- and it's the most important senior-level trick.

* * * *

4 is wrong --- and here's exactly why
-----------------------------------

You said: `call` passes `user`, so `this = user`, prints `"Vijay"`.

**Actual output: `undefined` (or `""` in browser)**

```
const arrowGreet = () => {
    console.log(this.name);
};

arrowGreet.call(user);   // call is IGNORED --- prints undefined

```

Arrow functions **cannot have their `this` overridden**. Ever. Not by `call`, not by `apply`, not by `bind`. The method exists on them, it just silently does nothing to `this`.

The arrow function was written at the top level --- outside any object, outside any regular function. So `this` at that scope is `window` (or `undefined` in strict mode). That's what it locked onto at birth. `call(user)` cannot change it.

* * * *

The exact mental model
----------------------

Think of it this way:

```
Regular function:  this is a PARAMETER --- caller can pass whatever they want
Arrow function:    this is HARDCODED at birth --- caller has zero control

```
```
// regular --- this is flexible, caller controls it
function greet() { console.log(this.name); }
greet.call(user);    // "Vijay" --- caller wins

// arrow --- this is frozen, caller is ignored
const greet = () => { console.log(this.name); }
greet.call(user);    // undefined --- call is silently ignored

```

* * * *

Why this matters in real code
-----------------------------

This is exactly why you must never use arrow functions as object methods:

```
const user = {
    name: "Vijay",

    greet: () => {              // ❌ arrow as method
        console.log(this.name); // this = window, not user
    },

    greet() {                   // ✅ regular method shorthand
        console.log(this.name); // this = user
    }
};

user.greet();   // arrow version → undefined
user.greet();   // regular version → "Vijay"

```

Arrow function was written inside the object literal but the object literal itself is not a scope --- it doesn't create a `this`. So the arrow looks past it to the outer scope, which is global.

* * * *

The complete rule set now in one place
--------------------------------------

| Scenario | Can `call/apply/bind` override `this`? |
| --- |  --- |
| Regular function | ✅ yes, fully |
| --- |  --- |
| Arrow function | ❌ no, silently ignored |
| `new` keyword | ❌ no, `new` always wins |

* * * *

One final boss question before we close this topic. No tricks, just synthesis:

```
class Button {
    constructor(label) {
        this.label = label;
    }

    handleClick() {
        console.log(this.label);
    }
}

const btn = new Button("Submit");

// scenario A
document.addEventListener("click", btn.handleClick);

// scenario B
document.addEventListener("click", btn.handleClick.bind(btn));

// scenario C
document.addEventListener("click", () => btn.handleClick());

```

Tell me --- A, B, C: what prints, and which two approaches are safe and why. This is a question that appears verbatim in senior React interviews.

Perfect --- all three correct. Now let me make sure the *why* is airtight, because knowing the answer isn't enough at senior level. They'll ask you to explain it.

* * * *

A --- `document.addEventListener("click", btn.handleClick)`
---------------------------------------------------------

```
document.addEventListener("click", btn.handleClick);

```

You're passing `btn.handleClick` as a reference. The dot is gone. When the browser fires the event, it calls the function like:

```
handler(event);   // plain call --- no object before the dot

```

Actually in DOM event listeners it's slightly more specific --- the browser sets `this` to the **element that fired the event**. So `this = document`, not `btn`.

```
handleClick() {
    console.log(this.label);   // this = document
}                              // document.label = undefined

```

Prints `undefined`. The method exists, it runs, but `this` is pointing at the wrong object entirely.

* * * *

B --- `.bind(btn)`
----------------

```
document.addEventListener("click", btn.handleClick.bind(btn));

```

`bind` creates a brand new function with `this` **permanently locked** to `btn`. The browser can't override it. Even though the browser tries to set `this = document`, bind already won that fight ahead of time.

```
handleClick() {
    console.log(this.label);   // this = btn, always, no matter who calls it
}

```

Prints `"Submit"`. This is the ES5 React pattern --- exact same mechanism as `this.handleClick = this.handleClick.bind(this)` in a constructor.

* * * *

C --- Arrow wrapper
-----------------

```
document.addEventListener("click", () => btn.handleClick());

```

The browser calls the arrow function. Inside it, `btn.handleClick()` is called with `btn` explicitly before the dot --- so `this = btn` at that call site. The arrow wrapper itself doesn't matter for `this` here. What matters is that you're calling the method correctly --- with the dot.

```
() => btn.handleClick()
//    ^^^
//    dot notation preserved --- this = btn inside handleClick

```

Prints `"Submit"`.

* * * *

Why B and C are both safe --- but differently
-------------------------------------------

```
B --- bind         locks this to btn at setup time, call site doesn't matter
C --- arrow wrap   doesn't lock this, but calls correctly every time via dot

```

Both arrive at the same result through different mechanisms:

```
// B: this is locked --- doesn't matter how it's called
btn.handleClick.bind(btn)   // new fn, this = btn forever

// C: this is free --- but call site is always correct
() => btn.handleClick()     // dot preserved, this resolved at call time

```

* * * *

The senior follow-up they always ask
------------------------------------

> "Between B and C, which is better and why?"

Both are valid. The real answer depends on context:

```
// B --- bind
// ✅ if you need to pass the function as a standalone reference
// ✅ slight performance edge (no wrapper function created on each render)
// ❌ easy to forget --- silent bug if you omit bind

// C --- arrow wrapper
// ✅ more readable, intention is obvious
// ✅ no need to remember bind
// ❌ creates a new function on every render in React
//    (causes unnecessary re-renders if passed as a prop)

```

In modern React with hooks, you avoid both problems entirely:

```
// hooks version --- no class, no bind, no wrapper issue
function Button({ label }) {
    const handleClick = useCallback(() => {
        console.log(label);   // label from closure, no this needed at all
    }, [label]);

    return <button onClick={handleClick}>{label}</button>;
}

```

`useCallback` memoizes the function so it's not recreated on every render. And since it's an arrow function closing over `label` directly, `this` is irrelevant entirely --- which is the real reason hooks felt like a liberation from `this` confusion.

* * * *

The complete mental model --- one paragraph
-----------------------------------------

`this` is determined at call time by who's calling the function. Object before the dot → that object. Plain call → window/undefined. `new` → fresh instance. Arrow → locked to outer scope at birth, immune to call/apply/bind. The entire ES5 React `.bind(this)` pattern exists because passing a method as a callback detaches it from its object, losing `this`. Bind restores it permanently. Arrow wrappers restore it at call time. Hooks sidestep the problem entirely by using closures instead of `this`.

* * * *

You've now covered the full `this` topic at senior depth. Want to move to the next frontend problem, or do you want me to throw some rapid-fire grilling questions the way an interviewer would --- no explanations, just you answering cold?

---
1. Return Value
Right now your throttle returns undefined when the function is dropped (cooldown). When it fires, it also returns undefined because you don't capture fn.apply()'s result.
The fix is one word — return:
jsfunction sixThrottle(fn, delay) {
  let lastExecutedDate = 0;
  return function (...args) {
    const curTime = Date.now();
    const curDelay = curTime - lastExecutedDate;
    if (curDelay >= delay) {
      lastExecutedDate = curTime;
      return fn.apply(this, args); // ← just add return
    }
    // dropped calls implicitly return undefined
  };
}
Why does this matter?
jsconst throttledAdd = throttle((a, b) => a + b, 1000);

const result = throttledAdd(2, 3);
console.log(result); // Without return: undefined. With return: 5
In UI work (scroll, resize) you rarely need the return value. But if throttle wraps a pure function someone is calling for its result, dropping the return silently breaks their code. Senior answer: "I'd always add return — zero cost, prevents surprises."

### 2\. Leading vs Trailing Edge

This is the most conceptually rich part. Right now your implementation is **leading edge only** --- fires at the *start* of each cooldown window, drops everything else.

```
Calls:    ↓  ↓  ↓  ↓  ↓        ↓  ↓  ↓
          |------ delay ------|  |------ delay ------|
Leading:  ✅ ✗  ✗  ✗  ✗        ✅ ✗  ✗
Trailing: ✗  ✗  ✗  ✗  ✅       ✗  ✗  ✅
Both:     ✅ ✗  ✗  ✗  ✅       ✅ ✗  ✅
```

#### Trailing edge --- fires at the END of the window

js
```
functionthrottleTrailing(fn, delay){let timer =null;returnfunction(...args){if(!timer){// No timer running? Start one      timer =setTimeout(()=>{        fn.apply(this, args);// Fire at end of window        timer =null;// Reset --- ready for next window}, delay);}// If timer exists: we're in cooldown --- dropped};}
```

> ⚠️ Trailing-only has a subtle bug: `args` is captured from the **first** call in the window, not the last. In practice you'd update `args` on every call. But for interviews, noting this awareness is enough.

### Functions are Objects in JavaScript

This is the key insight you're missing, and it's a beautiful JS quirk.

**In JavaScript, functions ARE objects.** They're a special type of object that happens to be callable. So you can attach properties to them just like any object.

js
```
functiongreet(){console.log("hello");}greet.city="Bangalore";// attach a property to a functiongreet.age=25;console.log(greet.city);// "Bangalore"console.log(greet.age);// 25greet();// "hello" --- still callable
```

`greet` is both a **callable function** AND an **object with properties**. Both at the same time.

* * * *

#### So `throttled.cancel` is just a property on a function

js
```
functionsixThrottle(fn, delay){let timer =null;functionthrottled(...args){// named function, not arrow// ... throttle logic}  throttled.cancel=function(){// attach .cancel as a propertyclearTimeout(timer);    timer =null;};return throttled;// return the function WITH .cancel attached}
```

When you call `sixThrottle(...)`, you get back `throttled` --- which is both:

-   **Callable** → `throttledFn(args)` works
-   **Has a property** → `throttledFn.cancel()` works

js
```
const throttledSave =sixThrottle(saveToServer,2000);throttledSave("data");// calls throttled()throttledSave.cancel();// calls the cancel function attached as property
```

* * * *