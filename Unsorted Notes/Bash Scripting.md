The syntax here applies to bash specifically. Other shells may have varying syntax, so check the documentation for those shells when needed.

Conditional Execution
- Format: if \[ cond \]; then
		    do stuff
		 else / elif \[ cond \]
		     do other stuff
		 fi
writing conditionals: \[ \$var {operator} {value} \]
- variables are expressed with a $ in front of their name
- The $ is not required for initial assignment, but you cannot have spaces in that line
- Spacing may or may not be important here
Arguments
- $0 is the script itself
- $1 - $9 are the 9 possible arguments
- Other special variables
	- $# - number of arguments passed to the script
	- $@ - list of command-line arguments
	- \$$ - process ID of the currently executing processing
	- $? - exit status of script (0 means successful execution, 1 means error)
Arrays
	- Declared using ()
	- Can be indexed using {} around how arrays are usually indexed
	- Elements separated by using spaces, and ''/"" are used to group many things as 1
Comparison Operators
	- variable to compare needs to be in double quotes
	- String operators
		- == - is equal to
		- != - is not equal to
		- < - less than in ASCII alphabetical order
		- > - greater than in ASCII alphabetical order
			- This and < have to be within double brackets to be used
		- -z - is empty/null
		- -n - is not empty/null
	- Integer operators
		- -eq - is equal to
		- -ne - is not equal to
		- -lt - is less than
		- -le - is less than or equal to
		- -gt - is greater than
		- -ge - is greater than or equal to
	- File operators
		- -e - does exist
		- -f - is a file
		- -d - is a directory
		- -L - is a symbolic link
		- -N - was modified after it was last read
		- -O - if current user owns the file
		- -G - if current user's group ID matches
		- -s - has a size greater than 0
		- -r - has read permissions
		- -w - has write permissions
		- -x - has execution permissions
		- The variable goes AFTER the operators here
	- Logical operators
		- ! - NOT
		- && - AND
		- || - OR
Arithmetic Operators
- Pretty much same as other languages
- ${#variable} - returns the length of variable
- use bc to perform any operations that involve floating points
Input/Output Control
```
read "display message here" variable_name
```
	- Add -p flag to specify that input remains on same line in bash
- Output control - use tee utility to direct output to files, instead of waiting for a command to finish
Loops
- Similar syntax to other languages
- For loops - need do/done before/after blocks of code in loop
```
for var in {start..end} / for var in {$collection}
do
# do things here
done
```
- While loop - same as usual
- Until loop - runs while a specified condition is false (opposite of while loop)
Case statements
```
case <expression> in
	pattern_1 ) statements ;;
	pattern_2 ) statements ;;
	pattern_3 ) statements ;;
esac
```
Functions in Bash
```
# Function Declaration
function name {
...
}
# Another way to declare functions: 
name() {
...
}
```
- To pass in arguments, one can either invoke them using $1, $2, $3, etc. for positional, or $variable if you want to pass a name.
	- Note - all variables are defined globally, unless the "local" tag is specified.
- Return Values
	- Always returns a code representing the status of execution, stored under $?
	- 0 = Execution OK
	- 1 = General errors
	- 2 = Misuse of shell builtins
	- 126 = Invoked command cannot execute
	- 127 = Command not found
	- 128 = Invalid argument to exit
	- 128 + n = Fatal error signal "n"
	- 130 = Script terminated by Control-C (force terminates a script)
	- 255\\* = exit status out of range
- Debugging Tips and Tricks
	- Invoke script using bash flags, ie bash -x -v script.sh
Dictionaries/Associative Arrays
- Check with shell to see if they are supported
- Bash 4.0+ supports dictionaries, zsh supports them baseline(?)
- Initialize with command declare -A dict-name
- If you already have some elements to insert, then do 
		declare -A dict-name=( \[key1\]=value1 \[key2\]=value2 \[key3\]=value3 ... )
	- Note - key/val pairs separated by spaces, use "" if a value or key requires spaces
- To insert an element -  dict-name\[key\] = value
	- Alternatively you can also do dict-name+= ( \[key\]=value )
- To access an element - ${dict-name\[key\]}
- To access a key - ${!dict-name\[key\]}
- To iterate over the array - ${dict-name\[@\]} (only iterates over values, use ! if you need keys)
- To get the length of the dictionary - ${#dict-name\[@\]}
- To check if a key exist in a dictionary - \[ $\{dict-name\[key\]\+\_} \]
- 