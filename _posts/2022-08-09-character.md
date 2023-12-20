---
layout: default
title:  "Character Set & Encoding"
categories: dev
date : 2022-08-09 20:09:40 +09:00
---

- [Character Set & Encoding](https://en.wikipedia.org/wiki/Universal_Coded_Character_Set) : To represent characters on a computer
    1. select characters
    1. represent as numbers(coded character set)
    1. represent as bits and bytes -> encoding

## Character set(Character map)
- ASCII : 7bits = 128 characters
- ANSI(American National Standard Institute) : ASCII + Code page 1bit. nonstandard.
- ISO/IEC 8859 : joint ISO and IEC series of standards for 8-bit character sets compatible with ASCII.
- KS X 1001(previous KS C 5601) : Korean double-byte
- Unicode(Universal Coded Character Set, UCS) : unicode.org
  - Standard set of characters defined by the international standard ISO/IEC 10646.
  - The Unicode Standard refers to the standard character set that represents all natural language characters.
  - All modern operating systems, computing environments, programming languages, and applications support the core of the Unicode Standard. 

## [Character Encodings](https://en.wikipedia.org/wiki/Character_encoding)
- ASCII(American Standard Code for Information Interchange) : 7bits encoding

### Multibytes encoding
> difficult to identify characters within a byte stream,\
> adaption of program from single byte to multibyte requires huge work,\
> different multibyte encodings require different adaptions.

- EUC-KR(extended unix code) : Korean 2bytes, English 1byte. W3C Standard. based on KS X 1001
- CP949(Code Page 949) : Unified Hangul Code by MS. based on KS X 1001

### Code switching
> even more difficult to handle than multibyte encodings,\
> difficult to get accurate information on some parts.

- ISO-2022-KR : RFC 1557(Korean Character Encoding for Internet Messages). It encodes ASCII and KS X 1001-1992.

### Encoding Unicode code points : 
1. UTF-8 : Universal Coded Character Set + Transformation Format
    - variable-length character encoding standard. 1~4Bytes. Korean 3 bytes
    - Code point <-> UTF-8 conversion

        | code point | Byte1  Byte2  Byte3  Byte4 |
        |--- | --- |
        | U+0000 ~ U+007F | 0xxxxxxx |
        | U+0080 ~ U+07FF	| 110xxxxx  10xxxxxx |
        | U+0800 ~ U+FFFF	| 1110xxxx  10xxxxxx  10xxxxxx |
        | U+10000 ~ U+10FFFF | 11110xxx  10xxxxxx  10xxxxxx  10xxxxxx |

    - UTF-8 encoding process ex.

        | Character	| Binary code point	| Binary UTF-8	| Hex UTF-8 |
        | --- | --- | --- | --- |
        | 한 | U+D55C | 1101 0101 0101 1100 | 11101101 10010101 10011100	| ED 95 9C |
        
    - As with most other encodings, UTF-8 is now preferred for new use, solving problems with consistency between platforms and vendors.
    - Usage
        - Default encoding for WEB(XML, HTML5) - space efficiency
        - Many protocols
        - IRI->URI conversion
        - Some file systems (getting popular for Unix/Linux file names)
        - Internal processing in some applications
1. UTF-16(extension of UCS-2)
    - U+0000 ~ U+FFFF
    - variable-length, as code points are encoded with one or two 16-bit code units.
    - Usage
        - Internal encoding for many applications and systems
        - Default in java
        - UTF-16 is more adequate with BMP (Basic Multilingual Plane) characters that can be represented with 2 bytes
1. UTF-32(previously named UCS-4) : 4bytes
    - Very simple and straightforward
    - Used on some Unix systems for internal processing
    - Space inefficient

1. Ref. BOM(byte order mark)\
A particular usage of the special Unicode character whose appearance as a magic number at the start of a text stream.

    - Unicode character encoding is used.
    - The byte order, or endianness (UTF-16,32)
        - UTF-8 BOM : U+EFBBBF
        - UTF-16 BOM : U+FEFF (Big Endian)
        - UTF-16 BOM : U+FFFE (Little Endian)
    - It might cause confusion in Linux system. (Program dependency)
    - JVM UTF-16 use Big Endian

### [ICU](https://unicode-org.github.io/icu/userguide/icu/)
Unicode’s programming libraries include ICU (International Components for Unicode).\
ICU is a mature, widely used set of C/C++(ICU4C) and Java(ICU4J) libraries providing Unicode and Globalization support for software applications.
1. Code Page Conversion : Convert text data to or from Unicode
1. Collation : Compare strings
1. Formatting: Format numbers, dates, times and currency amounts according the conventions of a chosen locale.
1. Time Calculations
1. Unicode Support
1. Regular Expression
1. Bidi(bi-directional language)
1. Text Boundaries: Locate the positions of words, sentences, paragraphs within a range of text, or identify locations that would be suitable for line wrapping when displaying the text.

## cf. 
- Base 64 Encoding
    - [rfc4648](https://datatracker.ietf.org/doc/html/rfc4648#section-4)
        - base 64 encoding은 character를 24비트의 이진데이터로 표시
        - US-ASCII 중 65개 글자를 이용하여, 6비트씩 나누어 의미를 부여
            | character | Base64 alphabet|
            |---|---|
            |A ~ Z|0 ~ 25|
            |a ~ z|26 ~ 51|
            |0 ~ 9|52 ~ 60|
            |+|62|
            |/|63|
            |(padding)|=|
        - URL Encoding, Email 등에 사용됨
    - [Why do we use base64](https://stackoverflow.com/questions/3538021/why-do-we-use-base64)
        >컴퓨터 간 전송 데이터는 이진법으로 인코드-디코드된다. 인코딩 방법은 여러가지라서, 장비마다 다르게 해석될 수 있다.
        >1Byte를 7bit로 처리할 수도 있고, 개행 문자로 \r\n 함께 사용될 수도 있다.
        
        >오류없이 안전하게 데이터를 교환하게 하기 위해, base64 인코딩이 제안되었다.

    -  [example](https://en.wikipedia.org/wiki/Base64)
        
        (1) M

        |ASCII Char|M| | | |
        |---|---|---|---|---|
        |ASCII Index|77| | | |
        |Bits| 0  1  0  0  1  1 | 0  1 (이후 padding) 0 0 0 0 |(padding)|(padding)|
        |Base64 Index|19|16|=|=|
        |Base64 Char|T|Q|=|=|
        ----------------------------------
        
        (2) !

        |ASCII Char|!| | | |
        |---|---|---|---|---|
        |ASCII Index|33| | | |
        |Bits| 0  0  1  0  0  0 | 0  1 (이후 padding) 0 0 0 0 |(padding)|(padding)|
        |Base64 Index|8|16|=|=|
        |Base64 Char|I|Q|=|=|
