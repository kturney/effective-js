1. Know Which JavaScript You Are Using
  - Some js implementations provide a `const` keyword
  - We should opt into strict mode when possible
    - Some care must be taken with strict mode to not enable it on code that doesn't support it
2. Understand JavaScript's Floating-Point Numbers
  - `number` is the only numeric data type in js
  - `number` is always a double-precision floating-point number|64-bit IEEE 754|double
  - all bitwise operations implicitly convert `number` into a 32-bit integer
  - to avoid rounding errors when dealing with currencies, it is common to perform operations using the currency's smallest, denomination, i.e. cents for $
  - when using `number` as an integer, you must stay between 2^-53 and 2^53
3. Beware of Implicit Coercions
  - arithmetic operators will attempt to convert args to `number`s
  - the exception is `+`, which will either add `number`s or concat `string`s.
  - `+` breaks the tie in favor of `string`s when `number`s and `string`s are mixed
  - the only way to reliably check if a value is `NaN` is to check that it doesn't equal itself, due to strange IEEE requirements
  - ```javascript
    var a = NaN;
    a !== a;       // true
    var b = "foo";
    b !== b        // false
    var c = NaN;
    c === NaN;     // false
    ```
  - objects are implicitly converted to `string`s using their `toString()` method
  - objects are implicitly converted to `number`s using their `valueOf()` method
  - avoid implementing `valueOf()` unless your object really represents a numeric type, otherwise you will get unexpected results when concatenating with a string (like during logging) because js will choose `valueOf()` over `toString()`
  - there are exactly 7 *falsy* values in js: `false`, `0`, `-0`, `""`, `NaN`, `null`, and `undefined`
  - the best way to check for an `undefined` param in a function that takes `number`s is either `typeof a === "undefined"` or `a === undefined` (I prefer the 2nd option)
4. Prefer Primitives to Object Wrappers
  - js has 5 types of primitive values: `boolean`, `number`, `string`, `null`, and `undefined`
  - `typeof` will confusingly report that `null` is of type `"object"`
  - object wrappers around primitives (`String` and `Number`) check reference equality, not value equality
  - calling functions or properties on a primitive value implicitly convert them into a wrapper object
  - the implicit conversion allows the strange behavior of setting a property on a primitive with no effect
  - property setting on primitives can lead to strange bugs when one does ot realize the value if a primitive and not an object
5. Avoid Using `==` with Mixed Types
  - the `==` operator applies a confusing set of implicit coercions when its arguments are of different types
  - it is good practice to use the `===` strict operator so that readers can easily tell no implicit conversion is expected
  - if conversion is needed, do it explicitly
6. Learn the Limits of Semicolon Insertion
  - semicolons can be omitted in certain contexts thanks to *automatic semicolon conversion*
  - Rules of semicolon insertion:
    1. Semicolons are only ever inserted before a } token, after one or more newlines, or at the end of the program input, i.e. at the end of a line, block, or program
    2. Semicolons are only ever inserted when the next input token cannot be parsed
      - you always have to check the start of the next statement to detect whether you can legally omit a semicolon
      - you can't leave off a statement's semicolon if the next line's initial token could be interpreted as a continuation of the statement
      - there are 5 problematic characters: (, [, +, - and /
      - a rule sometimes used to protect against the problematic characters is to prefix lines starting with a pc with a `;`
      - js will auto insert semicolons after `return`, `throw`, `break`, postfix `++`, and postfix `--`; so arguments must be on the same line to be interpreted correctly
    3. Semicolons are never inserted as separators in the head of a for loop or as empty statements
7. Think of Strings as Sequences of 16-bit Code Units
  - if a character is represented by a code point higher than 2^16 then it will throw off string functions (like `length` and `charAt`) because it will be reported as 2 characters
8. Minimize Use of the Global Object
  - avoid declaring global variables
  - declare variables as locally as possible
  - avoid adding properties to the global object
  - use the global object for platform feature detection
9. Always Declare Local Variables
  - Always declare new local variables with `var`
  - Consider using lint tools to help check for unbound variables
10. Avoid `with`
  - `with` binds all properties of the object and *all of its prototypes* to the scope
  - trying to use a variable from outside the `with` scope that has the same name as a property on the `with` object can cause unexpected results because the object's property will be used
  - there are pretty much no good cases in which to use `with`
11. Get Comfortable with Closures
  - 
