---
title:  "Effective JavaScript"
subtitle: "提升功力的心法秘籍"
author: "wu"
avatar: "img/authors/timg10.jpg"
image: "img/timg10.jpg"
date:   2018-10-07 15:00:00
---

Effective JavaScript

## 1 - Accustoming Yourself to JavaScript

### Item 1: Know Which JavaScript You Are Using

> Decide which versions of JavaScript your application supports.

> Be sure that any JavaScript features you use are supported by all environments where your application runs.

> Always test strict code in environments that perform the strict-mode checks.

> Beware of concatenating scripts that differ in their expectations about strict mode.

### Item 2: Understand JavaScript's Floating-Point Numbers

> JavaScript numbers are double-precision floating-point numbers.

> Integers in JavaScript are just a subset of doubles rather than a separate datatype.

> Bitwise operators treat numbers as if they were 32-bit signed integers.

> Be aware of limitations of precisions in flaoting-point arithmetic.

### Item 3: Beware of Implicit Coercions

> Type errors can be silently hidden by implicit coercions.

> The + operator is overloaded to do addition or string concatenation depending on its argument types.

> Objects are coerced to numbers via valueOf and to strings via toString.

> Objects with valueOf methods should implement a toString method taht provides a string representation of the number produced by valueOf.

> Use typeof or comparison to undefined rather than truthiness to test for undefined values.

### Item 4: Prefer Primitives to Object Wrappers

> Object wrappers for primitive types do not have the same behavior as their primitive calues when compared for equality.

> Getting and setting properties on primitives implicitly creates object wrappers.

### Item 5: Avoid using == with Mixed Types

> The == operator applies a confusing set of implicit coercoins when its arguments are of different types.

> Use === to make it clear to your readers that your comparison does not involve any implicit coercions.

> Use your own explicit coercions when comparing values of different types to make your program's behavior clearer.

### Item 6: Learn the Limits of Semicolon Insertion

> Semicolons are only ever inferred before a }, at the end of a line, or at the end of a program.

> Semicolons are only ever inffered when the next token cannot be parsed.

> Never omit a semicolon before a statement beginning with (, [, +, -, or /.

> When concatenating scripts, insert semicolons explicitly between scripts.

> Never put a newline before the argument to return, throw, break, continue, ++, or --.

> Semicolons are never inferred as separators in the head of a for loop or as empty statements.

### Item 7: Think of Strings As Sequences of 16-Bit Code Units

> JavaScript strings consist of 16-bit code units, not Unicode code points.

> Unicode code points 2^16 and above are represented in JavaScript by two code units, known as a surrogate pair.

> Surrogate pairs throw off string element counts, affecting length, charAt, charCodeAt, and regular expression patterns such as ".".

> Use third-party libraries for writing code point-aware string manipulation.

> Whenever you are using a library that works with strings, consult the documentation to see how it handles the full range of code points.

--------------- 华丽的配图分界线 ---------------

> Effective JavaScript - Loi Wu -

<div class="scale"><img src="img/timg10.jpg"  alt="Effective JavaScript" /></div>



