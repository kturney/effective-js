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
  - objects are implictly converted to `string`s using their `toString()` method
  - objects are implictly converted to `number`s using their `valueOf()` method
  - avoid implementing `valueOf()` unless your object really represents a numeric type, otherwise you will get unexpected results when concating with a string (like during logging) because js will choose `valueOf()` over `toString()`
  - there are exactly 7 *falsy* values in js: `false`, `0`, `-0`, `""`, `NaN`, `null`, and `undefined`
  - the best way to check for an `undefined` param in a function that takes `number`s is either `typeof a === "undefined"` or `a === undefined` (I prefer the 2nd option)
4. Prefer Primitives to Object Wrappers
  - js has 5 types of primitive values: `boolean`, `number`, `string`, `null`, and `undefined`
  - `typeof` will confusingly report that `null` is of type `"object"`
  - object wrappers around primitives (`String` and `Number`) check reference equality, not value equality
  - calling functions or properties on a primitive value implictly convert them into a wrapper object
  - the implicit conversion allows the strange behavior of setting a property on a primitve with no effect
  - property setting on primitives can lead to strange bugs when one does ot realize the value if a primitive and not an object
5. Avoid Using `==` with Mixed Types
  - the `==` operator applies a confusing set of implict coercions when its arguments are of different types
  - it is good practice to use the `===` strict operator so that readers can easily tell no implict conversion is expected
  - if conversion is needed, do it explictly
6. Learn the Limits of Semicolon Insertion
  - semicolons can be omitted in certain contexts thanks to *automatic semicolon conversion*
  - Rules of semicolon insertion:
    1. Semicolons are only ever inserted before a } token, after one or more newlines, or at the end of the program input, i.e. at the end of a line, block, or program
    2. Semicolons are only ever inserted when the next input token cannot be parsed
      - you always have to check the start of the next statement to detect whether you can legally omit a semicolon
      - you can't leave off a statement's semicolon if the next line's initial token could be interpreted as a continuation of the statement
      - there are 5 problematic characters: (, [, +, - and /
      - a rule sometimes used to protect against the problematic characters is to prefix lines starting with a pc with a `;`
      - js will auto insert semicolons after `return`, `throw`, `break`, postfix `++`, and postfix `--`; so arguments must be on the same line to be interpretted correctly
    3. Semicolons are never inserted as seperators in the head of a for loop or as empty statements
