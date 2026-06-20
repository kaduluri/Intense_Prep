```js
//1. Implement `var`/`let`/`const` behavior examples and explain output.
function oneVarImpl(){
    console.log(a);//hoisting of var a
    //console.log(b);//hoisting of let b but Ref error due TDZ
    var a=3;
    let b=4;
    const c=5;
    //c=6;//will result in error due to reassignment of const
    for(let i=0;i<4;i++){
        setTimeout(function(){
            console.log(i);//block scope of let ensures each setTimeout prints different value of i
        },1000)
    }
}
//oneVarImpl();
//Key: Chrome Snippets auto-print the return value of the last expression
//oneVarImpl() has no return → prints "undefined". Normal console doesn't do this.
//due to setimeout delay of 1s first function return of undefined printed firsrt and then setimeout output is printed

//----------------------------------

//2. Write `isEmptyObject(obj)` without using JSON.stringify.
function twoIsEmptyObject(obj){
    //console.log(Object.getOwnPropertyNames(obj).length > 0 ? "Not Empty Object":"Empty Object");
    return Object.getOwnPropertyNames(obj).length === 0;

    //different ways of common approaches:
    // 1. Object.keys (most common/idiomatic)
    //return Object.keys(obj).length === 0
    
    // 2. Object.values (works but semantically odd — you care about keys, not values)
    //return Object.values(obj).length === 0
    
    // 3. Object.entries
    //return Object.entries(obj).length === 0 //entries returns key,value pairs
    
    // 4. Object.getOwnPropertyNames (your solution — also checks non-enumerable properties)
    //return Object.getOwnPropertyNames(obj).length === 0
    
    // 5. for...in loop (old school, also catches inherited properties)
    /*function isEmptyObject(obj) {
        for (let key in obj) {
            if (obj.hasOwnProperty(key)) return false
        }
        return true
    }*/
    
    // 6. JSON.stringify
    //return JSON.stringify(obj) === "{}"
}
console.log(twoIsEmptyObject({a:1,b:2}));
//twoIsEmptyObject({});
```

//--------------------------------------

```js
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
```
The core concept: property attributes
-------------------------------------

Every property on a JS object secretly carries **descriptor flags** behind the scenes, not just a value:

```
// When you write this:
const obj = { a: 1 }

// JS internally stores it as:
{
  a: {
    value: 1,
    enumerable: true,    // shows up in loops/Object.keys?
    writable: true,      // can value be changed?
    configurable: true   // can property be deleted/redefined?
  }
}

```

`enumerable` is the flag we care about here.

* * * *

Analogy: A hotel room
---------------------

Think of an object as a **hotel room**. Properties are **items in the room**.

| Item | Type | Visible to guest? |
| --- |  --- |  --- |
| Bed, TV, minibar | Enumerable | ✅ Yes --- you can see and use them |
| --- |  --- |  --- |
| Hidden safe behind painting | Non-enumerable | ❌ No --- exists but not advertised |
| Amenities from hotel policy (inherited) | Inherited | 🏨 Belongs to hotel, not your room |

-   **`Object.keys`** → only lists what the guest can see (enumerable own items)
-   **`getOwnPropertyNames`** → lists everything physically in the room, including the hidden safe
-   **`for...in`** → lists your room items AND hotel-wide amenities (inherited)

* * * *

Non-enumerable properties --- what and why
----------------------------------------

```
const obj = { a: 1 }

// Manually define a hidden (non-enumerable) property
Object.defineProperty(obj, 'secret', {
    value: 42,
    enumerable: false   // 👈 hidden from normal iteration
})

console.log(obj.secret)              // 42 --- it EXISTS
console.log(Object.keys(obj))        // ['a'] --- secret is INVISIBLE
console.log(Object.getOwnPropertyNames(obj)) // ['a', 'secret'] --- catches it!

```

`Object.keys` skips `secret` entirely. `getOwnPropertyNames` finds it.

**Where do non-enumerable properties appear in real life?**

```
// Array's 'length' is non-enumerable, in Object length doesn't appear in getOwnProprtyNames but for Arrays it does appear because length is builtin propert for arrays and not Object
const arr = [1, 2, 3]
Object.keys(arr)               // ['0', '1', '2'] --- no 'length'
Object.getOwnPropertyNames(arr) // ['0', '1', '2', 'length'] --- length appears!

// All built-in methods on prototypes are non-enumerable
// e.g. toString, hasOwnProperty --- that's why they don't show up when you loop objects

```

JS intentionally hides built-in plumbing as non-enumerable so it doesn't pollute your loops.

* * * *

Inherited properties --- the prototype chain
------------------------------------------

Every object in JS secretly links to a **parent object** called its prototype. Properties on that parent are "inherited."

```
const animal = {
    breathes: true    // defined on parent
}

const dog = Object.create(animal)
dog.name = 'Rex'      // defined on dog itself

console.log(dog.name)     // 'Rex'    --- own property
console.log(dog.breathes) // true     --- inherited from animal

```

Now watch how each method handles this:

```
Object.keys(dog)                  // ['name'] --- own enumerable only
Object.getOwnPropertyNames(dog)   // ['name'] --- own only (enumerable + non-enumerable)

for (let key in dog) {
    console.log(key)              // 'name', 'breathes' --- includes inherited!
}

// for...in sees up the chain, keys/getOwnPropertyNames stop at the object itself

```

**Hotel analogy mapped:**

-   `dog.name` = item you personally put in your room (**own**)
-   `dog.breathes` = hotel policy that applies to every room (**inherited**)
-   `Object.keys` = only your stuff
-   `for...in` = your stuff + hotel's stuff

* * * *

What "catching" means for isEmptyObject
---------------------------------------

```
const obj = {}

Object.defineProperty(obj, 'ghost', {
    value: 99,
    enumerable: false
})

// Is this object truly empty?
Object.keys(obj).length === 0              // true  ← misses ghost!
Object.getOwnPropertyNames(obj).length === 0  // false ← catches ghost!

```

`Object.keys` says "nothing here" but the object secretly has a property. Whether that matters depends on your use case:

| Scenario | Use |
| --- |  --- |
| Checking user data, API responses, config objects | `Object.keys` --- non-enumerable props won't exist here |
| --- |  --- |
| Checking internal/library objects | `getOwnPropertyNames` --- safer, more thorough |
| Interview answer | `Object.keys` --- expected standard answer |

* * * *

Mental model summary
--------------------

```
Object.keys()               → own + enumerable only        (what you normally see)
Object.getOwnPropertyNames() → own + enumerable + hidden   (everything in the room)
for...in                    → own + inherited + enumerable (your room + hotel policy)

```

Think of it as **three expanding circles** --- each one sees more than the previous.

---

## Currying
* * * *

What happens at `const add = curry((a, b, c) => a + b + c)`
-----------------------------------------------------------

`curry(fn)` is called. The function body runs:

```
function curry(fn) {
  const arity = fn.length;  // arity = 3

```

Then it hits `return function curried(...args) { ... }` --- **this entire function is handed back and stored in `add`.**

Nothing else runs. Just that one `return`.

So right now:

```
arity = 3   (alive in closure)
fn    = (a,b,c) => a+b+c   (alive in closure)

add   = function curried(...args) {
          if (args.length >= arity) {
            return fn(...args);
          } else {
            return function(...moreArgs) {
              return curried(...args, ...moreArgs);
            };
          }
        }

```

`add` is literally that whole function sitting idle. **Nothing inside it has run yet.**

* * * *

`add(1)` --- curried runs for the first time
------------------------------------------

```
function curried(...args) {   // args = [1]

```
```
args   = [1]
arity  = 3
1 >= 3?  NO  → goes to else

```

else block runs:

```
return function(...moreArgs) {
  return curried(...args, ...moreArgs);
};

```

This creates a **brand new function** and returns it. That new function has `args = [1]` frozen in its closure.

So `add(1)` gives you back:

```
function(...moreArgs) {
  return curried(1, ...moreArgs);  // 1 is frozen
}

```

Call this `f1`. It's just sitting there now.

* * * *

`add(1)(2)` --- means `f1(2)` --- the inner function runs
-----------------------------------------------------

```
function(...moreArgs) {   // moreArgs = [2]
  return curried(...args, ...moreArgs);
  //     curried(1, 2)
}

```

`f1` runs. It calls `curried(1, 2)`.

Now `curried` runs again --- second time:

```
args   = [1, 2]
arity  = 3
2 >= 3?  NO  → goes to else

```

else block runs again, creates another new function:

```
function(...moreArgs) {
  return curried(1, 2, ...moreArgs);  // 1,2 frozen
}

```

Call this `f2`. `add(1)(2)` gives you `f2`.

* * * *

`add(1)(2)(3)` --- means `f2(3)` --- inner function runs again
----------------------------------------------------------

```
function(...moreArgs) {   // moreArgs = [3]
  return curried(...args, ...moreArgs);
  //     curried(1, 2, 3)
}

```

`f2` runs. It calls `curried(1, 2, 3)`.

`curried` runs --- third time:

```
args   = [1, 2, 3]
arity  = 3
3 >= 3?  YES  → if block runs

```
```
return fn(...args);
// fn(1, 2, 3)
// (a,b,c) => a+b+c
// 6

```

* * * *

Your specific question --- "we have 3 returns, what does add have"
----------------------------------------------------------------

```
function curry(fn) {
  const arity = fn.length;

  return function curried(...args) {        // ← RETURN 1
    if (args.length >= arity) {
      return fn(...args);                   // ← RETURN 2
    } else {
      return function(...moreArgs) {        // ← RETURN 3
        return curried(...args, ...moreArgs);
      };
    }
  };
}

```
```
RETURN 1 --- runs when curry(fn) is called
           add gets this --- the whole curried function
           happens ONCE

RETURN 2 --- runs when args are finally enough
           gives back the actual result (6)
           this is the EXIT

RETURN 3 --- runs when args are NOT enough yet
           gives back a collector function
           happens as many times as needed

```

`add` only ever holds what **RETURN 1** gave it. The other two returns happen later, each time `add(...)` or the resulting functions are called.

---

### 1\. Rewriting with a regular function inside setTimeout

js
```
// ARROW version (your current code)timerId =setTimeout(()=>{    fn.apply(this, args);}, delay);// REGULAR function versiontimerId =setTimeout(function(){    fn.apply(this, args);// ❌ WRONG --- `this` is now the wrong thing}, delay);
```

The problem: a regular function creates its own `this`. Inside `setTimeout`, that `this` is either `window` (in browsers) or `undefined` (in strict mode) --- not the `this` from the outer returned function.

So you have to capture `this` manually before entering the regular function:

js
```
returnfunction(...args){const self =this;// capture outer thisclearTimeout(timerId);    timerId =setTimeout(function(){        fn.apply(self, args);// use saved reference, not `this`}, delay);}
```

An arrow function has no `this` of its own --- it inherits it from the surrounding scope. That's exactly why arrow functions were invented for callbacks like this. The `const self = this` pattern is the old pre-ES6 workaround for the same problem.