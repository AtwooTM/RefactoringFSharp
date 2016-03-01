## "Refactoring F#"

Examples from my "Refactoring F#" talk.


## Key principles of functional programming

* Separate code from data
* Every function has an output -- this is important for composition
* Functions should not have hidden side-effects

## Functional Design Guidelines

* Composition everywhere
  * E.g. Convert everything to the same "level" using lifting - then composition is easier.
* Parameterize all the things
  * E.g. remove dependencies on global code by passing in functions 
* There is no problem that cannot be solved by wrapping it in a type
* Model the system like a compiler: 
  1. Parse the (untrusted) input into a trusted internal representation (use Option.ofObj, ofNullable, etc)
  2. Process and transform the data
  3. Emit data back to the untrusted world: transform domain types back to strings and ints.


## Tips for refactoring from imperative code

* Replace throwing exceptions with returning a Result. Chain results together using "bind". See http://fsharpforfunandprofit.com/rop/
* Change void-returning methods to return something.
* Use types to remove the need for validation. Obviously no nulls, but also e.g PositiveInteger
  * Pass the buck! It's the caller's responsibility to give you valid data
* Replace inheritance with discriminated unions where appropriate.
* Replace if/then/else tests with pattern matching -- it helps you detect edge cases
* Use Active patterns to make conditional logic clearer

## Tips for converting imperative loops

* If you are iterating over all the elements, use fold
* If you need to break early, use recursion, or fold with a flag.
* Take full advantage of the built-in collection functions -- see http://fsharpforfunandprofit.com/posts/list-module-functions/
* Don't forget `choose` and `pick`
* Don't treat lists like indexed collections


## General Tips

* Using Some/None matching with options can generally be replaced with Option.map or Option.bind.
* Use #IComparable, etc., instead of ugly type constraints
* Booleans
  * If you see boolean flags in a data structure, chances are it's a state machine. Use a DU instead.
  * If you see a function returning a boolean, replace the result with something useful.


## Code smells

* Ignoring the output of an expression
* Throwing exceptions rather than returning error types.
* Returning unit rather than something useful.
* Null checks in the core domain -- these should only be done at the boundary
* Wildcards in pattern matching
* Primitive obsession: Using strings and ints everywhere, rather than exploiting the type system.
* Accessing "global" functions and state rather than having them be passed in as parameters
* Treating lists like indexed collections
* Treating lists as appendable collections
* Over-reliance on "if" expressions and booleans rather than using pattern matching.
* Over-reliance on booleans (see tip above)
* Unwrapping and rewrapping options using "IsSome" rather than using staying in the world of options and using Option.map and Option.bind.
  * See http://fsharpforfunandprofit.com/series/map-and-bind-and-apply-oh-my.html
* Having deeply nested lambdas - I prefer the use of private helpers

