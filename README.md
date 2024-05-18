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
