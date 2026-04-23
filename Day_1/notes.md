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