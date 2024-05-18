# Cone
C++ with CamelCase style  
C++ without semicolons  
C++ with simplified syntax

- **Cone**, COne, cOne, c1, C1,  
  Pronounced „see one“
- „Improved“ C++
    - with a **simplified** syntax,
    - in the _style_ of Qt, Objective-C, Java, JavaScript, Kotlin, Swift
    - Isomorphic mapping of all C++ functionality to Cone possible
        - only with other/better/shorter „expression“.
    - ~~May I call this "modern"?~~
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
            - SharedPtr<T> etc.

- **Compatible to C++**, C and maybe other languages of this „language family“
    - as with
        - Java: Kotlin, Scala, Groovy, Clojure, Fantom, Ceylon, Jython, JRuby …
        - C#: C++/CLI, F#, Visual Basic .NET, Ada, IronPython, IronRuby …
        - Objective-C: Swift
    - Possible to include
        - C++ headers and modules from Cone
        - Cone headers and modules from C++
    - Language is recognised by
        - the file extension
            - C++: *.cpp *.hpp *.cxx *.hxx *.h
            - C: *.c *.h
                - *.h is of course a problem, but can probably best be solved using path based rules.
            - Cone: *.cone *.hone *.c1 *.h1 *.co *.ho
        - path based rules
            - C++ standard headers in certain directories 
        - can be set in IDE
            - for each file individually 

## Style
- All types and standard classes in upper CamelCase
    - Style similar to Kotlin, Swift
    - Cone standard library („cone::“ instead of „std::“)
        - „std::string“ -> „cone::String“
        - „vector“ -> „Vector“
        - „map“ -> „Map“
            - „Dictionary“ as typedef with deprecation warning
        - „forward_list“ -> „ForwardList“
        - „unordered_map“ -> „UnorderedMap“
        - „value_type“ -> „ValueType“
        - Maybe some exceptions/variations:
            - „stringstream“ -> „Stringstream“ or „StringStream“?
                - „Textstream“ or TextStream“, „Bytestream“ or „ByteStream“, …
            - „multimap" -> „Multimap“ or „MultiMap“?
    - Arithmetic types
        - Bool
        - Int == Int32 or 64
            - Int8, Int16, Int32, Int64, maybe Int128
        - UInt == UInt32 or 64
            - UInt8, UInt16, UInt32, UInt64, maybe UInt128
        - BigInt (Arbitrary Precision Integer)
        - Float
            - Float16, Float32, Float64 (Half, Single, Double Precision Floating Point)
                - maybe Float128, Float256
            - BFloat16 (Brain Floating Point)
            - BigFloat for Arbitrary Precision Float
        - Byte == UInt8
        - Char == UInt8, CodePoint == UInt32

- Functions in lower camelCase
    - Roughly in the style of Qt, Objective-C/C++, Java, JavaScript, TypeScript, Kotlin, Swift

- Namespaces fully lowercase 
    - Standard namespace „cone“~~, „c1“ ~~

## Arithmetic Types
- Bool
    - not ~~bool~~ nor ~~Boolean~~
- Int, UInt
    - Int == Int64
        - Int == Int32 on 32 bit systems only (i.e. old/small platforms)
            - therefore it is _not_ necessary to have ~~Size or SSize~
    - Int8, Int16, Int32, Int64, maybe Int128, Int256
        - like int32_t or qint32, but no prefix „q“ nor postfix „_t,) and in CamelCase 
    - UInt8, UInt16, UInt32, UInt64, maybe UInt128, UInt256
        - e.g. UInt256 for SHA256
- Byte == UInt8 (Alias)
- BigInt – Arbitrary Precision Integer
    - for cryptography, maybe computer algebra, numerics
    - see [Boost.Multiprecision](https://www.boost.org/doc/libs/1_79_0/libs/multiprecision/doc/html/index.html), [GMP](https://gmplib.org)
- Float
    - Float == Float32
        - Among other things because this is how it works in C/C++
        - Is faster than Float64 and good enough most of the time
    - ~~Float == Float64~~
        - ~~Among other things because already in C/C++ „1.0“ == Float64 and „1.0f“ == Float32~~
        - ~~Float32 only on certain platforms~~
    - Float16, Float32, Float64 (half, single, double precision floating point)
        - maybe Float128, Float 256
            - typically probably realized as double-double respectively double-double-double-double
            - https://stackoverflow.com/a/6770329
    - BFloat16 (Brain Floating Point)
    - Mixed arithmetic:
        - „1 * aFloat“ is possible
            - Warning, if the Int literal cannot be reproduced exactly as Float32/64
        - „anInt * aFloat“ is possible
            - Warning that the Int variable may not be reproduced exactly as Float32/64, i.e. with
                - aFloat32 * anInt32/64    // Warning
                - aFloat32 * anInt8/16     // No warning
                - aFloat64 * anInt64       // Warning
                - aFloat64 * anInt8/16/32  // No warning
    - BigFloat for arbitrary precision float,
        - see [GMP](https://gmplib.org)
        - But where do algorithms stop whose results cannot be represented precisely?
            - For example: „1.0 / 3.0“

## Signed Size
Int (i.e. signed) as type for *.size()
- Because mixed integer arithmetic („signed - unsigned“) and „unsigned - unsigned“ is difficult to handle.
    - In C/C++ „anUInt - 1 >= 0“ is _always_ true (even if anUInt is 0)
- When working with sizes, calculating the difference is common; Then you are limited to PtrDiff (i.e. signed integer) anyway.
- Who needs more than 2GB of data in an „array“, should please use a 64 bit platform.
- For bounds checking, the two comparisons „x >= 0“ and  „x < width“ may very well be reduced to a single „UInt(x) < width“ _by the compiler_ in an optimization step. 
- Then types Size and SSize/PtrDiff are not necessary anymore, so two types less.
    - We simply use Int instead. Or UInt rate cases.
- Restricted rules for mixed integer arithmetic:
    - Unsigned +-*/ Signed is an error
        - you have to cast
        - Int (i.e. signed) is almost always used anyways
    - Error with „if (anUInt < 0)“
        - if the literal on the right is <= 0
    - Error with „if (anUInt < anInt)“
        - you have to cast
- Not:
    - ~~Size AKA UInt as type for *.size() (i.e. still unsigned)~~
        - ~~Size reads IMHO better than UInt~~
            - ~~Size would not really be necessary~~
        - ~~New rules for mixed integer arithmetic:~~
            - ~~Unsigned +-*/ Signed -> Signed.~~
                - ~~Signed is therefore considered the "larger" type compared to unsigned~~
                - ~~„1“ is Int (signed)~~
                    - ~~„1u“ is UInt (unsigned)~~
                - ~~Therefore „if (anUInt - 1 >= 0)“ is a useful expression („1“ is signed)~~
                - ~~But also „anUInt + 1 == anInt“~~
    - ~~Or~~
        - ~~Size - Size -> SSize~~
            - ~~Problem: „-“ results in SSize, but „+“ results in Size?!~~
        - ~~The conversion of a negative number into Size lead to an error instead of delivering a HUGE size ~~
