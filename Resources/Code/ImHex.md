---
tags:
  - software
  - utility
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/WerWolv/ImHex](https://github.com/WerWolv/ImHex)

![[ImHexLogoSVGBG.svg]]

A Hex Editor for Reverse Engineers, Programmers and people who value their retinas when working at 3 AM.

## Screenshots
![[403520355-902a7c4c-410d-490f-999e-14c856fec027.png]]

![[403523006-58eefa1f-31c9-4bb8-a1c1-8cdd8ddbd29f.png]]

## Features
**Featureful hex view**
- Byte patching
- Patch management
- Infinite Undo/Redo
- "Copy bytes as..."
    - Bytes
    - Hex string
    - C, C++, C#, Rust, Python, Java & JavaScript array
    - ASCII-Art hex view
    - HTML self-contained div
- Simple string and hex search
- Goto from start, end and current cursor position
- Colorful highlighting
    - Configurable foreground highlighting rules
    - Background highlighting using patterns, find results and bookmarks
- Displaying data as a list of many different types
    - Hexadecimal integers (8, 16, 32, 64 bit)
    - Signed and unsigned decimal integers (8, 16, 32, 64 bit)
    - Floats (16, 32, 64 bit)
    - RGBA8 Colors
    - HexII
    - Binary
- Decoding data as ASCII and custom encodings
    - Built-in support for UTF-8, UTF-16, ShiftJIS, most Windows encodings and many more
- Paged data view

**Custom C++-like pattern language for parsing highlighting a file's content**

- Automatic loading based on MIME types and magic values
- Arrays, pointers, structs, unions, enums, bitfields, namespaces, little and big endian support, conditionals and much more!
- Useful error messages, syntax highlighting and error marking
- Support for visualizing many different types of data
    - Images
    - Audio
    - 3D Models
    - Coordinates
    - Time stamps

**Theming support**

- Doesn't burn out your retinas when used in late-night sessions
    - Dark mode by default, but a light mode is available as well
- Customizable colors and styles for all UI elements through shareable theme files
- Support for custom fonts

**Importing and Exporting data**

- Base64 files
- IPS and IPS32 patches
- Markdown reports

**Data Inspector**

- Interpreting data as many different types with endianness, decimal, hexadecimal and octal support and bit inversion
    - Unsigned and signed integers (8, 16, 24, 32, 48, 64 bit)
    - Floats (16, 32, 64 bit)
    - Signed and Unsigned LEB128
    - ASCII, Wide and UTF-8 characters and strings
    - time32_t, time64_t, DOS date and time
    - GUIDs
    - RGBA8 and RGB65 Colors
- Copying and modifying bytes through the inspector
- Adding new data types through the pattern language
- Support for hiding rows that aren't used

**Node-based data pre-processor**

- Modify, decrypt and decode data before it's being displayed in the hex editor
- Modify data without touching the underlying source
- Support for adding custom nodes

**Loading data from many different data sources**

- Local Files
    - Support for huge files with fast and efficient loading
- Raw Disks
    - Loading data from raw disks and partitions
- GDB Server
    - Access the RAM of a running process or embedded devices through GDB
- Intel Hex and Motorola SREC data
- Process Memory
    - Inspect the entire address space of a running process

**Data searching**

- Support for searching the entire file or only a selection
- String extraction
    - Option to specify minimum length and character set (lower case, upper case, digits, symbols)
    - Option to specify encoding (ASCII, UTF-8, UTF-16 big and little endian)
- Sequence search
    - Search for a sequence of bytes or characters
    - Option to ignore character case
- Regex search
    - Search for strings using regular expressions
- Binary Pattern
    - Search for sequences of bytes with optional wildcards
- Numeric Value search
    - Search for signed/unsigned integers and floats
    - Search for ranges of values
    - Option to specify size and endianness
    - Option to ignore unaligned values

**Data hashing support**

- Many different algorithms available
    - CRC8, CRC16 and CRC32 with custom initial values and polynomials
        - Many default polynomials available
    - MD5
    - SHA-1, SHA-224, SHA-256, SHA-384, SHA-512
    - Adler32
    - AP
    - BKDR
    - Bernstein, Bernstein1
    - DEK, DJB, ELF, FNV1, FNV1a, JS, PJW, RS, SDBM
    - OneAtTime, Rotating, ShiftAndXor, SuperFast
    - Murmur2_32, MurmurHash3_x86_32, MurmurHash3_x86_128, MurmurHash3_x64_128
    - SipHash64, SipHash128
    - XXHash32, XXHash64
    - Tiger, Tiger2
    - Blake2B, Blake2S
- Hashing of specific regions of the loaded data
- Hashing of arbitrary strings

**Diffing support**

- Compare data of different data sources
- Difference highlighting
- Table view of differences

**Integrated disassembler**

- Support for all architectures supported by Capstone
    - ARM32 (ARM, Thumb, Cortex-M, AArch32)
    - ARM64
    - MIPS (MIPS32, MIPS64, MIPS32R6, Micro)
    - x86 (16-bit, 32-bit, 64-bit)
    - PowerPC (32-bit, 64-bit)
    - SPARC
    - IBM SystemZ
    - xCORE
    - M68K
    - TMS320C64X
    - M680X
    - Ethereum
    - RISC-V
    - WebAssembly
    - MOS65XX
    - Berkeley Packet Filter

**Bookmarks**

- Support for bookmarks with custom names and colors
- Highlighting of bookmarked region in the hex editor
- Jump to bookmarks
- Open content of bookmark in a new tab
- Add comments to bookmarks

**Featureful data analyzer and visualizer**

- File magic-based file parser and MIME type database
- Byte type distribution graph
- Entropy graph
- Highest and average entropy
- Encrypted / Compressed file detection
- Digram and Layered distribution graphs

**YARA Rule support**

- Scan a file for vulnerabilities with official yara rules
- Highlight matches in the hex editor
- Jump to matches
- Apply multiple rules at once

**Helpful tools**

- Itanium, MSVC, Rust and D-Lang demangler based on LLVM
- ASCII table
- Regex replacer
- Mathematical expression evaluator (Calculator)
- Graphing calculator
- Hexadecimal Color picker with support for many different formats
- Base converter
- Byte swapper
- UNIX Permissions calculator
- Wikipedia term definition finder
- File utilities
    - File splitter
    - File combiner
    - File shredder
- IEEE754 Float visualizer
- Division by invariant multiplication calculator
- TCP Client/Server
- Euclidean algorithm calculator

**Built-in Content updater**

- Download all files found in the database directly from within ImHex
    - Pattern files for decoding various file formats
    - Libraries for the pattern language
    - Magic files for file type detection
    - Custom data processor nodes
    - Custom encodings
    - Custom themes
    - Yara rules

**Modern Interface**

- Support for multiple workspaces
- Support for custom layouts
- Detachable windows

**Easy to get started**

- Support for many different languages
- Simplified mode for beginners
- Extensive documentation
- Many example files available on [the Database](https://github.com/WerWolv/ImHex-Patterns)
- Achievements guiding you through the features of ImHex
- Interactive tutorials

## Pattern Language
The Pattern Language is the completely custom programming language developed for ImHex. It allows you to define structures and data types in a C-like syntax and then use them to parse and highlight a file's content.

- Source Code: [Link](https://github.com/WerWolv/PatternLanguage/)
- Documentation: [Link](https://docs.werwolv.net/pattern-language/)

## Database
For format patterns, libraries, magic and constant files, check out the [ImHex-Patterns](https://github.com/WerWolv/ImHex-Patterns) repository.

**Feel free to PR your own files there as well!**

