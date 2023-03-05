# The Go Programming Language Specification, according to JavaScript
This is a parody of this [document](https://go.dev/ref/spec), from a JavaScript developer's perspective. It is recommended to read this and that side-by-side. Currently, this document is updated according to version December 15, 2022 of that document.

## Introduction
This is the reference manual for the Go programming language, much like how the [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) is, de-facto, for JavaScript. The pre-Go1.18 version, without generics, can be found [here](https://go.dev/doc/go1.17_spec.html). You probably don't know generics, so you might want to read that instead unless you're coming from TypeScript. For more information and other documents, see [golang.org](https://golang.org).

Go is a general-purpose language designed with systems programming in mind (which all Node.js devs wish was JavaScript). It is strongly typed and garbage-collected and has explicit support for concurrent programming. Programs are constructed from packages, whose properties allow efficient management of dependencies, similar to how packages/modules are in JavaScript. Maybe without the "efficient" part (if you're using [pnpm](https://pnpm.io) then you're exempted from this quip).

The syntax is compact and simple to parse, allowing for easy analysis by automatic tools such as integrated development environments. On this regard, Go and proper TypeScript are similar (emphasis on **proper**).

## Notation
It's highly likely you haven't got a clue what Extended Backus-Naur Form is. To save you from massive nasal blood loss, the syntax will be described using a mix of plain English and JavaScript.

Here's a tip: if you want to be able to effectively read formal language specifications like this, you will want to acquaint yourself with metasyntaxes like [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form). It also comes in handy when reading [RFCs](https://www.ietf.org/standards/rfcs/).

## Source code representation
All you need to know about source code representation in Go can be summarized in this bulleted list:
- Like JavaScript, source code in Go is encoded in [UTF-8](https://en.wikipedia.org/wiki/UTF-8).
- Each code point is distinct; like how ``A`` is from ``a``, and ``;`` is from ``;``.
- Just don't use the NUL character (U+0000) in the source text. Why would you even need to?
- Why would a sane programmer manually put a UTF-8 encoded BOM (byte order mark) at the very start of the code file?

### Characters
If you have any business being interested in this section, you should read it in the original specification.

### Letters and digits
The underscore character _ (U+005F) is considered a lowercase letter.

Other than that, in Go:
- A letter is a Unicode letter.
- A decimal digit is either ``0``, ``1``, ``2``, ``3``, ``4``, ``5``, ``6``, ``7``, ``8``, or ``9``.
- A binary digit is either ``0``, or ``1``.
- An octal digit is either ``0``, ``1``, ``2``, ``3``, ``4``, ``5``, ``6``, or ``7``.
- A hexadecimal digit is either ``0``, ``1``, ``2``, ``3``, ``4``, ``5``, ``6``, ``7``, ``8``, ``9``, ``a``, ``b``, ``c``, ``d``, ``d``, ``e``, ``f``, ``A``, ``B``, ``C``, ``D``, ``E``, or ``F``.

## Lexical elements

### Comments
Comments serve as program documentation. There are two forms:

1. Line comments are similar to JavaScript line comments (``//``).
2. General comments are, likewise, similar to JavaScript block comments (``/* */``).

A comment cannot start inside a rune (you will get to know this later) or string literal, or inside a comment. A general comment containing no newlines acts like a space. Any other comment acts like a newline.

TL;DR: comments in Go are basically the same as comments in JavaScript.

### Tokens
Tokens form the vocabulary of the Go language. There are four classes: identifiers, keywords, operators and punctuation, and literals. White space, formed from spaces (U+0020), horizontal tabs (U+0009), carriage returns (U+000D), and newlines (U+000A), is ignored except as it separates tokens that would otherwise combine into a single token. Also, a newline or end of file may trigger the insertion of a semicolon (similar to JavaScript). While breaking the input into tokens, the next token is the longest sequence of characters that form a valid token.

If you didn't grok that paragraph, just bear in mind that Go syntax is made up of symbols and keywords, like any other programming language syntax.

### Semicolons
Like JavaScript, semicolons (``;``) are used to terminate code statements.

Go programs may omit semicolons according to these rules:
1. A semicolon is auto-inserted after a line's final token, but only if that token is falls under several categories. If you want to know what these categories are, just know that this auto-semicolon-after-new-line mechanism is identical to JavaScript's. If you still want to know what these categories are, then just ditch this document and read the official one.
2. A semicolon can be omitted before a closing ``}`` or ``)``, similar to how you can do ``() => { a++; return }`` in JavaScript one-liners.

This specification recommends that you don't type semicolons when coding Go (and leave that business to linters).

### Identifiers
Identifiers in Go are identical to identifiers in JavaScript. An identifier is a sequence of one or more letters and digits. The first character in an identifier must be a letter.

Some identifiers are [predeclared], which, in JavaScript lingo, means they already exist in the global scope. It's like how you can use ``null`` or ``true`` or ``false`` without seeing them anywhere in your code file.

### Keywords
The following keywords are reserved and may not be used as identifiers.
````
break           default         func        interface       select
case            defer           go          map             struct
chan            else            goto        package         switch
const           fallthrough     if          range           type
continue        for             import      return          var
````

If you want a comparison to the list of reserved words from JavaScript (excluding the future ones), here you go:

````
break       continue        else        for             instanceof      switch      typeof      let
case        debugger        export      function        new             this        var         static
catch       default         extends     if              null            throw       void        yield
class       delete          false       import          return          true        while       await
const       do              finally     in              super           try         with
````

### Operators and punctuation
The following character sequences represent [operators](#operators) (including [assignment operators](#assignment-statements)) and punctuation:
````
+    &     +=    &=     &&    ==    !=    (    )
-    |     -=    |=     ||    <     <=    [    ]
*    ^     *=    ^=     <-    >     >=    {    }
/    <<    /=    <<=    ++    =     :=    ,    ;
%    >>    %=    >>=    --    !     ...   .    :
     &^          &^=          ~
````

If you want a comparison to the list of operators and punctuation in JavaScript, don't be lazy and read this [resource](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators).

### Integer literals
Integer literals in Go are similar to numeric literals in JavaScript. Don't take this statement for granted; compare this section's original text to this [MDN page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#numeric_literals).

There's literally no difference except for BigInt and that floating-point and exponential literals are also treated as numeric literals. It's as if the Go language authors wanted Go to be easy to learn for JavaScript developers.

### Floating-point literals
Feel free to skip this section and treat floating-point numbers like how JavaScript treats them: not important.

If you feel otherwise, probably it's part of the reason why you switched over to Go. Below's the original text of this section, heavily annotated with comparisons to JavaScript's system for you to understand it quicker.

A floating-point literal is a decimal or hexadecimal representation of a floating-point constant (in JavaScript, the numeric exponential literal performs the same role).

A decimal floating-point literal consists of an integer part (decimal digits), a decimal point, a fractional part (decimal digits), and an exponent part (e or E followed by an optional sign and decimal digits). One of the integer part or the fractional part may be elided; one of the decimal point or the exponent part may be elided (same in JavaScript: ``1. || .5``). An exponent value exp scales the mantissa (integer and fractional part) by 10exp (likewise with JavaScript).

A hexadecimal floating-point literal consists of a 0x or 0X prefix, an integer part (hexadecimal digits), a radix point, a fractional part (hexadecimal digits), and an exponent part (p or P followed by an optional sign and decimal digits). One of the integer part or the fractional part may be elided; the radix point may be elided as well, but the exponent part is required (This syntax matches the one given in IEEE 754-2008 §5.12.3.). An exponent value exp scales the mantissa (integer and fractional part) by 2exp. There is no equivalent for this in JavaScript.

For readability, an underscore character _ may appear after a base prefix or between successive digits; such underscores do not change the literal value (this is also true for JavaScript).

Syntax-wise, decimal floating-point literals in Go are identical to their JavaScript counterparts. There is no point of comparison for hexadecimal floating-point literals in JavaScript, so you'll have to learn the syntax for them (it's not that hard, it's like a JavaScript numeric exponential literal with a hex literal for a mantissa and the exponent part preceded by "p").

````
0.          // you can do this in JS
72.40       // you can do this in JS
072.40      // == 72.40, and you can do this in JS
2.71828     // you can do this in JS
1.e+0       // you can do this in JS
6.67428e-11 // you can do this in JS
1E6         // you can do this in JS
.25         // you can do this in JS
.12345E+5   // you can do this in JS
1_5.         // == 15.0, and you can do this in JS
0.15e+0_2    // == 15.0, and you can do this in JS

/* no equivalent in JS */
0x1p-2       // == 0.25
0x2.p10      // == 2048.0
0x1.Fp+0     // == 1.9375
0X.8p-0      // == 0.5
0X_1FFFP-16  // == 0.1249847412109375
0x15e-2      // == 0x15e - 2 (integer subtraction)

0x.p1        // invalid: mantissa has no digits
1p-2         // invalid: p exponent requires hexadecimal mantissa
0x1.5e-2     // invalid: hexadecimal mantissa requires p exponent

/* also invalid in JS */
1_.5         // invalid: _ must separate successive digits
1._5         // invalid: _ must separate successive digits
1.5_e1       // invalid: _ must separate successive digits
1.5e_1       // invalid: _ must separate successive digits
1.5e1_       // invalid: _ must separate successive digits
````

### Imaginary literals
There is no counterpart for this in JavaScript. Majority of JS devs write for the web, not for the aerospace industry.

This doesn't mean imaginary numbers cannot be done in JavaScript. You just need strings or arrays to represent them and a library to do the computations (like [Complex.js](https://github.com/infusion/Complex.js/)).

### Rune literals
A rune literal represents a rune constant, an integer value identifying a Unicode code point. A rune literal is expressed as one or more characters enclosed in single quotes, as in 'x' or '\n'. Within the quotes, any character may appear except newline and unescaped single quote. A single quoted character represents the Unicode value of the character itself, while multi-character sequences beginning with a backslash encode values in various formats.

If you parsed that paragraph correctly, you'll know that there is no exact counterpart for this in JavaScript. The nearest would be either single character String literals, like `"a"`, or escaped sequences, like `"\x41"`. Although, the fact that Go runes are integer values, and that you cannot append runes directly to strings in Go, makes JavaScript numbers the technically correct equivalent. In terms of usage however, like for representing a Unicode code point in a literal form, JavaScript strings are a better equivalent.

As for rune literal formats, one can draw parallels between them and how characters can be specified in JavaScript strings. For further comparison, this [article](https://mathiasbynens.be/notes/javascript-escapes) by Matthias Bynens should help.

One thing that JS devs should be aware of is that you cannot append rune literals directly onto string literals. You'll have to typecast them first.

### String literals
A string literal represents a string constant obtained from concatenating a sequence of characters. There are two forms: raw string literals and interpreted string literals. You'll want to skim this part in the original specification first before reading the comparison to JavaScript so you can understand it better.

Once you're done skimming, you'll notice that it's almost the same in JavaScript except for a couple of important differences:
- In JavaScript, all possible string literals (as enclosed by `'` or `"` or `` ` ``) are interpreted, in contrast to Go where only double quote enclosed literals are interpreted. If you still didn't get what "interpreted" means, it means the source code interpreter checks the string literal for escaped characters.
- Creating raw string literals in JavaScript is not as convenient as it is in Go. You'll have to use the `String.raw` method to make them.