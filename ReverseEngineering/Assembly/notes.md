### General
* An instruction set for a CPU, describes its hardwired circuits that it uses to perform certain instructions
* Assembly code is a human-readable way of representing machine code
* An assemlber assembles the assembly code into machine code
** Assembly will not run on the processor directly, it needs to be assembled into machine code
* Characters like 'A' will get converted into their binary equivalent
* Without sections, everything runs down in assembly
* When you reference a label, it references the address in memory
** Ex: mov si, message
* 0eh is a command to the BIOS
* THE CODE RUNS DOWN. It does not care if you have data in the way, it will run it, unless you assemble it with instructions
** message: db 'HEllo world', 0 <-- The processor would attempt to run 'H' as an instruction, which is why you need to use a jmp to urn somewhere else.
* 0eh means "print one character to the screen"


# 8086 instructions
* org 100h, means to load into memory address 100 hex
** the program will start executing from there
** helps to make sure we offset correctly when we assemble something
** tells assembler "we'll always be loaded into this address" this allows it to ensure that everything is offset to this address
** Not an actual assemblt instruction, just tells the assembler to offset from there

* ret
** means we are returning from a subroutine
** turns control back to caller of the subroutine
** the final ret will return control back to the operating system

* mov
** means that we are moving data to some register

* int
** means interrupt
** Doing `int 10h` means let's call interrupt 10h.
** 10h (hexadecimal 10) is responsible for outputting to the screen
** 10h is a BIOS routine
*** BIOS actually deals with the hardware of getting whatever on the screen
** In the BIOS's kernel `int 10h` is responsible for outputting to the screen

* db 
** means data byte and it allows us to specify a number of characters

* jmp
** I think that you can use jmp to jump to a label

* lodsb
** Can load things and increments. 
** it loads into the al register, previously used to display the character

* cmp
** Can compare values

* je
** jump equal (I guess, jump if the previous cmp condition was true)

### Registers
* Storage units in the processor itself that can store a certain amount of data
** ah and al are registers. They each can store 1 byte. You could reference them both at once with ax

