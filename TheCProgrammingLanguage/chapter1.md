# The C programming Langauge (Second Edition) Notes

* In C89/90, the int return type is implied. If you want ANSI, then this standard is considered correct. In C99, however, this is no longer the case. 
```c
#include <stdio.h>

main() 
{
                printf("Hello world\n");
}
```
* The `#include <stdio.h>` line tells the compiler to include information about the standard I/O library. We then use the library function printf to display a sequence of characters to the screen.  


* We call things like "Hello world!\n" character strings or string constants.   


* In C, all variables must be declared before they are used. A declaration announces the properties of something, such as the type and name. In teh case of functions, it would also inlcude a list of arguments


* The range of certain variable types vary from machine to machine. It is common to see both 16-bit integers and 32-bit integers.  A floar number is typiclaly a 32-bit quantity, with at least siz significant digits and magnitude generally between 10⁻³⁸ and 10⁺³⁸.

## Basic Data types in C
* **int**               integer (whole number)
* **float**           floating point number (number with decimals
* **char**            character - a single byte
* **short**           short integer
* **long**             long integer
* **double**        double percision floating point

* The size of these basic data types is machine-dependent

* How while loops operate:
    * The condition in the parentheses is tested. If it is true (fahr is less than or equal to upper), the body of the loop (the statements enclosed in the braces) is executed. Then the condition is re-tested, and iif true, the body is executed again.


* In C, as in many other languages, integer operations truncate. For example, in C, the expression 5/9 would evaluate to 0 because the decimal component is chopped off.
    * If an arithmetic operator has integer operands, an integer operation is performed. However, if an arithmetic operator has one floating-point operand and integer operand, the integer will be converted to floating point before the operation is performed.


* You can augment format specifiers in calls to printf with a width. The width is by default 1 (nothing). So if you want something to be justified to the right by 1 unit then you will need to do %2d.  It takes into account the value of the thing being formatted.
```c
printf("%3d, %6d\n", 1, 2);
```
* When using the %f format specifier, you can include the number of fraction digits that you want.
    * Ex: %6.1f, %3.0f
    * Of course, both the width and the percision cna be omitted from a specification



* How for loops operate:
    * The first part of the for loop is the initialization, IT is done once before the looop is entered. 
    * The second part is the condition that controls the loop. It is evaluated, and if true, the body of the loop is executed.
    * Then the increment step occurs and the condition is checked agiain.


* When trying to choose between for loops and while loops, choose whichever looks cleaner. A for loop is generally better when the initialkization and incrment are single statements are are logically related.

## Character Input and Output
* The C standard library models text in a very simple manner, called text streams. Input and output, regardless of where it originates or goes to is reprented as a sequence of characters divided into lines; each line consists of zero or more characters followed by  a newline character. 
* it is the responsivility of the library to make each input or output stream conform to this model;
* The getchar function reads the next input character from a text stream and returns that as its value.
* The putchar function pprints a character each time. 

* The char data type is specifically meant for holding character data, but really, any integer type can be used.
* The getchar function returns a distictive value when there is no more inout, a value that cannot be confused with any real character.
    * This value is called EOF, for "end of file."
* If there is a possibility that a variable might which holds a single character might hold an EOF, then you need to use int because the char data type will not be big enough
* EOF is an **integer** defined in stdio.h, but the specific numeric value doesn't matter as long as it is not eh same as any char value.
    * By using the symbolic constant, we arew assured that nothing in the program depends on the specific numeric value.


