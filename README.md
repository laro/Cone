# Cone
C++ with CamelCase style  
C++ without semicolons  
C++ with simplified syntax

- **Cone**, COne, cOne, c1, C1,  
  Pronounced "see one"
- "Improved" C++
    - with a **simplified** syntax,
    - in the _style_ of Qt, Objective-C, Java, JavaScript, Kotlin, Swift
    - Isomorphic mapping of all C++ functionality to Cone possible
        - only with other/better/shorter "expression".
    - C++ Successor Language
        - like CppFront/Cpp2, Carbon or Circle
        - Similar to
            - C -> C++
            - Java -> Kotlin
            - Objective-C -> Swift
            - JavaScript -> TypeScrtipt
    - Uses the same compiler backend as C++ (clang, gcc, …)
        - with an own / a new compiler frontend (or a precompiler)
    - So _no_ garbage collection 
        - Instead use
            - RAII (Resource Acquisition is Initialization)
                - RROD (Resource Release on Object Destruction)
            - `SharedPtr<T>` etc.


- **Compatible to C++**, C and maybe other languages of this "language family"
    - as with
        - Java: Kotlin, Scala, Groovy, Clojure, Fantom, Ceylon, Jython, JRuby …
        - C#: C++/CLI, Visual Basic .NET, F#, A# (Ada), IronPython, IronRuby …
        - Objective-C: Swift
    - Possible to include
        - C++ headers and modules from Cone
        - Cone headers and modules from C++
    - Language is recognised by
        - the file extension
            - C++: `*.cpp` `*.hpp` `*.cxx` `*.hxx` `*.h`
            - C: `*.c` `*.h`
                - `*.h` is of course a problem, but can probably best be solved using path based rules.
            - Cone: `*.cone` `*.hone` `*.c1` `*.h1` `*.co` `*.ho`
        - path based rules
            - C++ standard headers in certain directories 
        - can be set in IDE
            - for each file individually 


## Style
- All types and standard classes in upper CamelCase
    - Style similar to Kotlin, Swift
    - Cone standard library (`cone::` instead of `std::`)
        - `std::string` -> `cone::String`
        - `vector` -> `Vector`
        - `map` -> `Map`
            - `Dictionary` as typedef with deprecation warning, as a hint for C# programmers.
        - `forward_list` -> `ForwardList`
        - `unordered_map` -> `UnorderedMap`
        - `value_type` -> `ValueType`
        - Maybe some exceptions/variations:
            - `stringstream` -> `Stringstream` or `StringStream`?
                - `Textstream` or `TextStream`, `Bytestream` or `ByteStream`, …
            - `multimap` -> `Multimap` or `MultiMap`?
    - Arithmetic types
        - `Bool`
        - `Int` == `Int32` or `64`
            - `Int8`, `Int16`, `Int32`, `Int64`, maybe `Int128`
        - `UInt` == `UInt32` or `64`
            - `UInt8`, `UInt16`, `UInt32`, `UInt64`, maybe `UInt128`
        - `BigInt` (Arbitrary Precision Integer)
        - `Float`
            - `Float16`, `Float32`, `Float64` (Half, Single, Double Precision Floating Point)
                - maybe `Float128`, `Float256`
            - `BFloat16` (Brain Floating Point)
            - `BigFloat` for Arbitrary Precision Float
        - `Byte` == `UInt8`
        - `Char` == `UInt8`, `CodePoint` == `UInt32`

- Functions in lower camelCase
    - Roughly in the style of Qt, Objective-C/C++, Java, JavaScript, TypeScript, Kotlin, Swift

- Namespaces fully lowercase 
    - Standard namespace `cone`~~, `c1` ~~


## Arithmetic Types
- `Bool`
    - not ~~`bool`~~ nor ~~`Boolean`~~
- `Int`, `UInt`
    - `Int` == `Int64`
        - `Int` == `Int32` on `32` bit systems only (i.e. old/small platforms)
            - therefore it is _not_ necessary to have ~~`Size` or `SSize`~~
    - `Int8`, `Int16`, `Int32`, `Int64`, maybe `Int128`, `Int256`
        - like `int32_t` or `qint32`, but no prefix "q" nor postfix "_t,) and in CamelCase 
    - `UInt8`, `UInt16`, `UInt32`, `UInt64`, maybe `UInt128`, `UInt256`
        - e.g. `UInt256` for SHA256
- `Byte` == `UInt8` (Alias)
- `BigInt` – Arbitrary Precision Integer
    - for cryptography, maybe computer algebra, numerics
    - see [Boost.Multiprecision](https://www.boost.org/doc/libs/1_79_0/libs/multiprecision/doc/html/index.html), [GMP](https://gmplib.org)
- `Float`
    - `Float` == `Float32`
        - Among other things because this is how it works in C/C++
        - Is faster than Float64 and good enough most of the time
    - ~~`Float` == `Float64`~~
        - ~~Among other things because already in C/C++ `1.0` == `Float64` and `1.0f` == `Float32`~~
        - ~~`Float32` only on certain platforms~~
    - `Float16`, `Float32`, `Float64` (half, single, double precision floating point)
        - maybe `Float128`, `Float256`
            - typically probably realized as double-double respectively double-double-double-double
            - https://stackoverflow.com/a/6770329
    - `BFloat16` (Brain Floating Point)
    - Mixed arithmetic:
        - `1 * aFloat` is possible
            - Warning, if the Int literal cannot be reproduced exactly as `Float32`/`64`
        - `anInt * aFloat` is possible
            - Warning that the Int variable may not be reproduced exactly as `Float32`/`64`, i.e. with
                - `aFloat32 * anInt32/64    // Warning`
                - `aFloat32 * anInt8/16     // No warning`
                - `aFloat64 * anInt64       // Warning`
                - `aFloat64 * anInt8/16/32  // No warning`
    - `BigFloat` for arbitrary precision float,
        - see [GMP](https://gmplib.org)
        - But where do algorithms stop whose results cannot be represented precisely?
            - For example: `1.0 / 3.0`


## Signed Size
`Int` (i.e. signed) as type for `*.size()`
- Because mixed integer arithmetic ("signed - unsigned") and "unsigned - unsigned" is difficult to handle.
    - In C/C++ `anUInt - 1 >= 0` is _always_ true (even if `anUInt` is `0`)
- When working with sizes, calculating the difference is common; Then you are limited to `PtrDiff` (i.e. signed integer) anyway.
- Who needs more than 2GB of data in an "array", should please use a 64 bit platform.
- For bounds checking, the two comparisons `x >= 0` and  `x < width` may very well be reduced to a single `UInt(x) < width` _by the compiler_ in an optimization step. 
- Then types `Size` and `SSize`/`PtrDiff` are not necessary anymore, so two types less.
    - We simply use Int instead. Or `UInt` in rate cases.
- Restricted rules for mixed integer arithmetic:
    - `Unsigned +-*/ Signed` is an error
        - you have to cast
        - `Int` (i.e. signed) is almost always used anyways
    - Error with `if (anUInt < 0)`
        - if the literal on the right is `<= 0`
    - Error with `if (anUInt < anInt)`
        - you have to cast
- Not:
    - ~~`Size` AKA `UInt` as type for `*.size()` (i.e. still unsigned)~~
        - ~~`Size` reads IMHO better than `UInt`~~
            - ~~`Size` would not really be necessary~~
        - ~~New rules for mixed integer arithmetic:~~
            - ~~Unsigned +-*/ Signed -> Signed.~~
                - ~~Signed is therefore considered the "larger" type compared to unsigned~~
                - ~~`1` is `Int` (signed)~~
                    - ~~`1u` is `UInt` (unsigned)~~
                - ~~Therefore `if (anUInt - 1 >= 0)` is a useful expression (`1` is signed)~~
                - ~~But also `anUInt + 1 == anInt`~~
    - ~~Or~~
        - ~~`Size` - `Size` -> `SSize`~~
            - ~~Problem: `-` results in `SSize`, but `+` results in `Size`?!~~
        - ~~The conversion of a negative number into `Size` leads to an error instead of delivering a HUGE size ~~


## String, Char & CodePoint
- `cone::String` with UTF-8 support
    - _Basic/standard_ unicode support
        - Iteration over:
            - code units (i.e. Bytes with UTF-8)
                - `for codeUnit in text.asArray()`
                - `for codeUnit in text.asCodeUnits()`?
                - ~~`for codeUnit in text.byCodeUnit()`?~~
                - ~~`for codeUnit in text.byChar()`?~~
            - code points (always `UInt32`, with UTF-8, UTF-16, and UTF-32)
                - `for codePoint in text.asCodePoints()`
                - ~~`for codePoint in text.byCodePoint()`?~~
            - grapheme clusters (i.e. may consist of multiple code points, default form of iteration, using `StringView`)
                - for graphemeCluster in `abc 🥸` (this is the default type of iteration)
                - additional/alternative names?
                    - `for graphemeCluster in text.asGraphemeClusters()`?
                    - ~~`for graphemeCluster in text.byGraphemeCluster()`?~~
    - Advanced support based on [ICU](https://unicode-org.github.io/icu/userguide/icu4c/) ("International Components for Unicode", "ICU4C").
        - "The ICU libraries provide support for:
            - The latest version of the Unicode standard
            - Character set conversions with support for over 220 codepages
            - Locale data for more than 300 locales
            - Language sensitive text collation (sorting) and searching based on the Unicode Collation Algorithm (=ISO 14651)
            - Regular expression matching and Unicode sets
            - Transformations for normalization, upper/lowercase, script transliterations (50+ pairs)
            - Resource bundles for storing and accessing localized information
            - Date/Number/Message formatting and parsing of culture specific input/output formats
            - Calendar specific date and time manipulation
            - Text boundary analysis for finding characters, word and sentence boundaries"
        - `import icu` adds extension methods for `cone::String`
            - Allows iteration over:
                - words (important/difficult for Chinese, Japanese, Thai or Khmer, needs list of words)
                    - `for word in text.asWords()`
                    - ~~`for word in text.byWord()`~~
                - lines
                    - `for word in text.asLines()`
                    - ~~`for line in text.byLine()`~~
                - sentences (needs list of abbreviations, like "e.g.", "i.e.", "o.ä.")
                    - `for sentence in text.asSentences()`
                    - ~~`for sentence in text.bySentence()`~~
    - `string.toUpper()`, `string.toLower()`
        - `toUpper(Sting)` -> `String`
        - `toLower(Sting)` -> `String`
    - Sorting
- `StringView`
    - to iterate over all grapheme clusters (i.e. may consist of multiple code points) of a string
        - `for graphemeCluster in "abc 🥸👮🏻"`
            - "a", "b", "c", " ", "🥸", "👮🏻"
            - "\x61", "\x62", "\x63", "\x20", "\xf0\x9f\xa5\xb8", "\xf0\x9f\x91\xae\xf0\x9f\x8f\xbb"
    - A bit slow, as it has to find grapheme cluster boundaries.
    - It is recommended to mostly use the standard functions for string manipulation anyway, you seldomly need grapheme-cluster-based iteration. But when you do, this probably is the correct way. 
- `CodePoint` == `UInt32`
    - to iterate over all code points of a string,
            - `for codePoint in "abc 🥸👮🏻".asCodePoints()`
            - 0x00000061, 0x00000062, 0x00000063, 0x00000020, &nbsp; 0x0001F978, &nbsp; 0x0001F46E, 0x0001F3FB 
    - Independent of the encoding (so, the same for UTF-8/16/32),
        - called "auto decoding" in D.
    - A bit faster, but still slow, as it has to find code point boundaries in UTF-8/16 strings.
    - Fast with UTF-32, **but** even with UTF-32 not all grapheme clusters fit into a single code point,
        - so not:
            - emoji with modifier characters like skin tone or variation selector,
            - diacritical characters (äöü…, depending on the normal form chosen),
            - surely some more …
        - Often slower than UTF-8, due to its size (cache, memory bandwidth)
- `Char` == `Char8`
    - to iterate over all code units (bytes/characters) of an UTF-8 string,
        - `for ch in "abc 🥸👮🏻".asArray()`
        - `for ch in "abc 🥸👮🏻"utf8.asArray()`
        - `for ch in UTF8String("abc 🥸👮🏻").asArray()`
            - 0x61, 0x62, 0x63, 0x20, &nbsp; 0xf0, 0x9f, 0xa5, 0xb8, &nbsp; 0xf0, 0x9f, 0x91, 0xae, 0xf0, 0x9f, 0x8f, 0xbb
    - to iterate over all bytes/characters of an ASCII string,
        - `for ch in "abc"ascii`
            - Compilation error, if string contains non-ASCII characters.
        - `for ch in ASCIIString("abc")`
            - Exception thrown, if string contains non-ASCII characters.
            - 0x61, 0x62, 0x63
            - ‚a‘, ‚b‘, ‚c‘
    - to iterate over all bytes/characters of a Latin1 (ISO 8859-1) string,
        - `for ch in "äbc"latin1`
            - Compilation error, if string contains non-Latin1 characters.
        - `for ch in Latin1String("äbc")`
            - Exception thrown, if string contains non-Latin1 characters.
            - 0xe4, 0x62, 0x63
            - ‚ä‘, ‚b‘, ‚c‘
- `Char16`
    - to iterate over strings encoded as UTF-16 with `.asArray()`
        - `for ch16 in "abc 🥸👮🏻"utf16.asArray()`
        - `for ch16 in UTF16String("abc 🥸👮🏻").asArray()`
            - 0x0061, 0x0062, 0x0063, 0x0020, &nbsp; 0xD83E, 0xDD78, &nbsp; 0xD83D, 0xDC6E, 0xD83C, 0xDFFB
- `Char32`
    - to iterate over strings encoded as UTF-32 with ".asArray()"
        - `for ch32 in "abc 🥸👮🏻"utf32.asArray()`
        - `for ch32 in UTF32String("abc 🥸👮🏻").asArray()`
            - 0x00000061, 0x00000062, 0x00000063, 0x00000020, &nbsp; 0x0001F978, &nbsp; 0x0001F46E , 0x0001F3FB
- `Char8`, `Char16`, `Char32`
    - are like `UInt8`, `UInt16`, `UInt32`,
    - but considered as _different_ types for parameter overloading.
- So no ~~`WideChar`~~
    - ~~Or is it useful for portable code (Linux `UInt32` <-> Windows `UInt16`)?~~
        - ~~You may use `wchar_t` then.~~


## Namespace `cone`
Standard namespace is `cone`~~, `c1`~~ (instead of `std`)
- With Cone version of every standard class/concept (i.e. uppercase class names and camelCase function and variable names)
    - e.g. `cone::String : protected std::string`
- "**Alias**" for 
    - member variables  
      `using x = data[0]`  
      `using y = data[1]`  
        - ~~or `alias x = data[0]`?~~
        - Not quite possible in C++.
            - With …  
              `Float& imaginary = im`  
              `T& x = data[0]`  
              … unfortunately memory is created for the reference (the pointer).
            - And this indeed is necessary here, because the reference could be assigned differently in the constructor,
                - so it is not possible to optimize it away.
    - member functions
        - `using f() = g()`
        - ~~Or is perfect forwarding enough?~~
            - ~~https://stackoverflow.com/a/9864472~~
            - This would not work for virtual functions


## Const Reference as Default Type
Const reference/value as default type for function call arguments and for "for-in" (AKA "for-each", "foreach").
- `mutable`, to mark as changeable
    - Also at the caller `swap(mutable a, mutable b)`
    - ~~Or `inout`?~~
- `value`, to mark as value (not reference) 
    - `mutable value`
    - Is not specified when calling the function, as a copy is created here.
- `reference`, to mark as reference (not value)
- RValue references still as `&&`
- Type traits `default_argument_type`
    - As const _value_:
        - `Int`, `Float`, `Bool` etc.
        - Small classes (as `Complex<Float>`, `StringView`) 
    - As const _reference_:
        - All other cases
    - Therefore probably best to have const reference as general default, "list of exceptions" for the "value types".
    - ~~Or (similar to C# and Swift) const-reference for `classes`, const-value for `structs`?~~
        - ~~At least as default?~~
- Examples:
    - `concat(String first, String second)`
        - instead of `concat(const String& first, const String& second)`
    - `String[] stringArray = ["a", "b", "c"]`  
      `for str in stringArray { … }`
        - `str` is `const String&`
        - If you want to have it differently:
            - `for mutable str in stringArray { … }`
                - `str` is `String&`
            - `for value str in stringArray { … }`
                - `str` is `const String`
            - `for mutable value str in stringArray { … }`
                - `str` is `String`
    - `for str in ["a", "b", "c"] { … }`
        - `str` is `const StringView`
    - `for i in [1, 2, 3] { … }`
        - `i` is `const Int`
        - If you want to have it differently:
            - `for mutable i in [1, 2, 3] { … }`
                - `i` is `Int`
            - `for reference i in [1, 2, 3] { … }`
                - `i` is `const Int&`
            - `for mutable reference i in [1, 2, 3] { … `}"
                - `i` is `Int&`
    - If you want even the basic type to be different:
        - `for Double d in [1, 2, 3] { … }`
            - `d` is `const Double`
        - `for String str in ["a", "b", "c"] { … }`
            - `str` is `const String&`
        - `for mutable String str in ["a", "b", "c"] { … }`
            - `str` is `String&`
        - `for value String str in ["a", "b", "c"] { … }`
            - `str` is `const String`
        - `for mutable value String str in ["a", "b", "c"] { … }`
            - `str` is `String`
    - `for i in 1..<10 { … }`
        - `i` is `const Int`


## Better Readable Keywords
C++ has a "tradition" of complicated keywords or reuse of keywords, simply as to avoid compatibility problems with old code, which may have used one of the new keywords as name (of a variable, function, class, or namespace).
- Cone has
    - `var` instead of `auto`
    - `func` instead of `auto`
    - `for … in …` instead of `for (… : …)`
        - `for i in 0..<10` instead of `for (int i = 0; i < 10; ++i)`
    - `class … extends …` instead of `class … : …`
        - or better `implements`?
    - `await` instead of `co_await`
    - `yield` instead of `co_yield`
    - `return` instead of `co_return`
    - `and`, `or`, `xor`, `not` instead of `&&`, `||`, `^`, `!`
        - as in Carbon, Python
        - Used for both
            - boolean operation
                - `anBool`**`and`**`anotherBool` -> `Bool`
            - bitwise operation
                - `anInt`**`and`**`anotherInt` -> `Int`
- When translating C++ code to Cone then change conflicting names, e.g.
    - `int var` -> `Int __variable_var`
    - `class func` -> `class __class_func`
    - `yield()` -> `func __function_yield()`
