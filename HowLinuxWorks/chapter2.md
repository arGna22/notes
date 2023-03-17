# How Linux Works

* There are many different unix shells, but all of them derrive from the Bourne Shell (/bin/sh)
* Commands that run in the shell could be both other programs or things that are built into the shell
* There was something on wildcards and I/O streams, go back and write about those.
* Unix processes use I/O streams to read and write data
* Processes read data from input streams and write data to output streams.
* Streams are very flexible. The source of an input stram could be a file, a device, a termianl window, or even the output stream from another process.
* The reason the cat command adopts an interactive behaviour when you do not specify an input filename is because it reads from the standard input stream provided by the kernel, rather than a stream connected to a file. In this case, stdin is connected to the terminal where you run cat.

### Shell globbing
* Can match simple patterns to file and directory names
* The shell matches arguments containing globs to  filenames, substitutes the filenames for those arguments, and then runs the revised command line.
    * The substitution is reffered to as expansion because the shell substitues all matching filenames for a simplified expression
* If no file matches a glob, the bassh shell performs no expansion, and the command runs with literal characters such as *.
* The shell performs expansions before running commands, and only then. Therefore, if a * makes it to a command without expanding, the shell won't do anything more with it. It is up to the command to decide what it wants to do.

### Intermediate UNIX commands
* grep
    * Prints lines from a file or input stream that match an expression
    * Very useful for operating on multiple files at once since it prints the filename in addition to the amthcing line
    * Use -i option for case insensitive matches
    * Use -v option for invert search (printing all of the lines that dont match)
    * More powerful varient exists called egrep, which is basically just grep -E
    * Understands regular expressions, which are more powerful than wildcard style patterns, adn they have a different syntax.
    * Regular expressions:
        * .* matches any number of characters, inclduign none liek the * in globs and wildcards
        * .+ matches any one or more characters
        * . matches exactly one arbitrary character.
* diff
    * See the difference between two text files
* find and locate
    * Finds files that are somewhere in the difrectory tree.
    * Accepts special pattern matching characters such as *, but you must enclose them in single quotes.
    * Most systes also have a locate command for finding files. Rather than searching for a file in real time, locate searches an index that the system builds periodically. Searching with locate is much faster than find, but if the file you're looking for is newer then the index, locate won't find it
    * Format for find: find dir -name file -print
* head and tail
    * Allow you to quicklu view a portion of a file or stream of data.
    * By default, head displays the first 10 lines of a file, and tail displays the last 10 lines. 
    * To change the number fo lines to display, use the -n option, where n is the number of lines you want to see (for example, head -5 /etc/passwd). 
    * To print lines starting at line n, use tail +n
* sort
    * Quickly puts the lines of a text file in alphanumeric order.
    * if the file's line start with numbers and you want to sort in numerical order, use the -n option.
    * The -r option reverses the order of the sort
## Changing your password and shell
* Use the passwd command to change your password
* Change your shell with the chsh command
## Dot files
* Configuration files/directories
* Shell globs don't match dot files unless you explicity use patterns such as .*
    * However, you can run into problems with globs because .* matches . and .. (the current and parent directories). Yoiu may wish to use a pattern such as .[^.]* or .??* to get all dot files escept he current and parent directories
## Environment and Shell Variables
* shell can store temporary variables, called shell variables, containing the values of text strings.
* Shell variables are very useful for keeping track of values in scripts, and some shell varaibles control the way she shell behaves
* TO assign a value to a shell variable, use the equal sign
    * Ex: STUFF=blah
    * Do not put any spaces.
* An environment variable is like a shell variable, but it si not specific to the shell
* The main difference between environment and shell variables is tha tth eoperating system passes all of your shell's enviromnment variables to programs that the shell runs, whereas shell varaibles cannot be accessed in the commands that you run.
* You assign environment variables with the shell's export command.
    * Ex: STUFF=blah && export STUFF
* Child processes inherit environment variables from their parent
* Many programs read from environment variables for configuration options.
### Getting online help
    * Use man -k to search a manual page by keyword
    * Manual pages are referenced by numbered sections
    * When someone refers to a manual page, they often put the  number in parentheses, like ping(8)
    * By default, man displays the first page that it finds, but you can select a manual page by section
        * man <section> <keyword>
    * Some time ago, the GNU Project decided that it didn't like the manual pages very much and switches to another format called info (or textinfo)
        * Often this documentation goes further than tuypical man pages, but it can be more complex
