# learning-avr
personal repository with stuff related to learning avr microcontroller

## Useful Links

- [Microchip - ATmega32A](https://www.microchip.com/en-us/product/ATmega32A)
- [Microschip - AVR Instruction Set](https://ww1.microchip.com/downloads/en/devicedoc/atmel-0856-avr-instruction-set-manual.pdf)
- [AVR Dude Manual](https://www.nongnu.org/avrdude/user-manual/avrdude.html)
- [AVR Dude Source Code](https://github.com/avrdudes/avrdude/)
- [USBasp](https://www.fischl.de/usbasp/)
- [USBasp Source Code](https://github.com/stv0g/usbasp)
- [ATmega32A IO Definitionc for C/C++](https://github.com/vancegroup-mirrors/avr-libc/blob/master/avr-libc/include/avr/iom32a.h)
- [GNU C Compiler Internals](https://en.wikibooks.org/wiki/GNU_C_Compiler_Internals/GNU_C_Compiler_Architecture)
- [Arduino Source Codes](https://github.com/arduino/ArduinoCore-avr)
- [Blog - AVR Dude Options](https://www.ladyada.net/learn/avr/avrdude.html)
- [Blog - AVR Programming in HEX](https://nuft.github.io/avr/2015/08/02/avr-hex-programming.html)


## FAQ

### How to compile C/C++ AVR code from command line

```
avr-gcc -Os -mmcu=atmega32a -c -o main.o main.c
avr-gcc -mmcu=atmega32 main.o -o main.el
avr-objcopy -O ihex -R .eeprom main.elf main.hex

avrdude -c usbasp -p atmega32a -P usb -v -U flash:w:main.hex
```

avr-gcc options:

| Option | Description |
| --- | --- |
| -c | compile or assemble the source files, but do not link |
| -S | stop after the stage of compilation proper, do not assemble |
| -E | stop after the preprocessing stage |
| -Os | optimize for size |
| -v | print commands |

For more options check man page or this links:
- [Default Options](https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html)
- [AVR Options](https://gcc.gnu.org/onlinedocs/gcc/AVR-Options.html)

### Can I programm AVR microcontrollers on MacOS

Yes, all tools needed to compiler and program AVR chips are available for MacOS as command line tools.
Microship Studio IDE is not available for MacOS at the moment.

Best way to install avr toolchain is to use homebrew package maneger.

```
brew install avrdude
xcode-select --install
brew tap osx-cross/avr
brew install avr-gcc
```

### What is Intel HEX format

Accordigng to [wikipedia](https://en.wikipedia.org/wiki/Intel_HEX):

Intel HEX is a file format that conveys binary information in ASCII text form, making it possible to store on non-binary media such as paper tape, punch cards, etc., to display on text terminals or be printed on line-oriented printers. The format is commonly used for programming microcontrollers, EPROMs, and other types of programmable logic devices and hardware emulators.

### How to disassemble AVR intel HEX file

To disassemble HEX file, you can use avr-objdump util that should come with avr-gcc & avr-libc.

```
avr-objdump -D -s -m avr5 main.hex 
```


### How to declare CPU Clock Speed

Some C/C++ functions, like `_delay_ms()` require preprocesor const F_CPU to be declared.

You can declare it inside source code or as compiler parameter.

```
#define F_CPU 1000000
```

```
avr-gcc -Os -DF_CPU=16000000UL -mmcu=atmega32a -c -o main.o main.c
```


### How to decompile obj file

You can decompile and disassemble object file to see C/C++ source file with corresponding assembly code.
To do that, you need to compile program with -g flag, and then use avr-objdump with -S and -l flags.

```
avr-gcc -Os -mmcu=atmega32a -g -o main.o main.c
avr-objdump -S -l -m avr5 main.o
```

| Option | Description |
| --- | --- |
| -S | display source  code  intermixed  with  disassembly,  if  possible |
| -l | Label  the  display (using debugging information) with the filename and source line numbers corresponding to the object code or  relocs shown |