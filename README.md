rekkrunchy
==========

This is my fork of rygs kkrunchy_k7 0.23a4/asm07, with patches.

Executable download: http://scene.org/file.php?file=%2Fresources%2Fcode%2Futils%2Frekkrunchy_030.zip&fileinfo

I renamed the project to avoid confusion with rygs original version.

So far everything works the same, except that pdb loading and size reports work again. 

Feel free to get in touch at ralph@deadfeed.net

*WARNING:* You need NASM version 2.10.07 for this to compile.

# Changes by BoyC / Conspiracy:

* added .kkp export for byte exact pack ratio, analyzer tool to be released soon, file format described below	
* fixed PE header so that the Microsoft exe signing tool actually recognizes produced binaries as executables
* As the result of the PE header fix, expanded the MZ header with some custom art

# KKP file format:
Used to describe a binary file with all its contents and compression statistics, including symbol info

```
4 bytes: FOURCC: 'KK64'
4 bytes: size of described binary in bytes (Ds)
4 bytes: number of source code files (Cc)

// source code descriptors:
Cc times:
	ASCIIZ string: filename
	float: packed size for the complete file
	4 bytes: unpacked size for the complete file, in int

4 bytes: number of symbols (Sc)

// symbol data:
Sc times:
	ASCIIZ string: symbol name
	double: packed size of symbol
	4 bytes: unpacked size of symbol in bytes
	1 byte: boolean to tell if symbol is code (true if yes)
	4 bytes: source code file ID
	4 bytes: source code line ID

// binary compression data:

Ds times: (for each byte of the described binary)
	1 byte: original data from the binary
	2 bytes: symbol index
	double: packed size
	2 bytes: source code line
	2 bytes: source code file index
```