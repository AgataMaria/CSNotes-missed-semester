
Official Notes: [https://missing.csail.mit.edu/2020/shell-tools/](https://missing.csail.mit.edu/2020/shell-tools/)

## Intro

_From Wiki:_  
> A **shell script** is a computer program designed to be **run by the Unix shell**, a command-line *interpreter*. The **various dialects of shell scripts are considered to be scripting languages**. ... Related programs such as shells based on Python, Ruby, C, Java, Perl, Pascal, Rexx &c in various forms are also widely available.

So in shell script you can use variables, for-loops etc.

### Variables
To _create vars_ you just declare and assign them -->
`foo=bar` # **DO NOT USE SPACES**   

To _access foo_ you use a `$` --> `$foo`   

For **string interpolation/substitution** you need to use double quotes -->  
`"Foo has a value of $foo"` CORRECT  
`'Foo has a value of $foo'` WRONG  

### Functions
To declare a function use '**fname** **()** **{** _functionbody_ **}**'  
To grab arguments from the command line instead of argv use `$numer` to interpolate to your function body  
_example_

	mcd () {
		mkdir -p "$1"
		cd "$1" 
	}

## Using shell script
:question::sweat_smile: whats vim?  

### Argument shorthands
**$0** - name of the script   
**$_** - last argument of the previous cmd  
**$#** - number of arguments  
**$@** - all arguments  
**!!** - last cmd  
**$?** - error from the previous cmd  
**$$** - cmd process id  
**{range}** - eg. `touch foo{1,9}` will touch foo1, foo2, foo3... foo9.  
also works with letters, but you use .. instead of , - `mkdir user{A..Z}` would create dirs userA, userB, userC... user Z  
can combine and do `touch {foo,bar}/{a..c}`  
**?** - placeholder for a single number eg. `dir?` could equate to anything in range dir0-dir0, dir10+ would be ignored  
** \* ** - wildcard, can be replaced by any number of characters - eg. `*.jpg` would mean anything that ends in '.jpg'  



### Logic
**||**  
logical or (execute right side if left side fails)

**&&**  
logical and (only executer right side if left side is true)

**;**  
'concatenate' - execute commands in order, regardless if successfull or not

**$(cmd)**  
command substitution  
`foo=$(pwd)` --> store the result of pwd in foo

**<(cmd)**  
process substitution
:question::sweat_smile: dont get it :D

**-ne**  
not equals

:question::sweat_smile: /dev/null  <-- special device in UNIX? need to read more

15:00 didn't get the grep "$file" > /dev/null 2> /dev/null <-- what did he mean baout 2>?

**if**  
if statements are written slightly differently...

	if [[ "$?" -ne 0]]; then
		# commands

so this reads if error from previous command is not equal zero (if there _is_ an error... otherwise command would print 0) then _do sth_  

## shell tools - good to know

### beyond bash
You can write scripts in a lot of different languages - eg. python  
\- in python you need to `import sys` as python does not interact with system by default
\- to grab cmd arg in python use `argv[number]` ($number in bash)  

\- to run a scrip in python from shell you have two options:  
	1. `python [path]` to use python and parse 'path' as an argument
	2. add `#!use/bin/env python` to find 'python' in 'env'

### shellcheck - bash
`shellcheck [path]` let's you check your script - it gives warnings and advisories for best practices

**NB.** running vs. loading bash
running scripts in isolation and loading shell have their differences  
:question::sweat_smile: not sure what he means? ~27min  

### manual & tldr
#### man
`man [cmd]` is super useful for checking command usage and options  
remembers it's a programm so you need to exit it with 'q' before you can go back to shell  

Reading man pages can be tricky, but you can use [tldr](#tldr)

#### tldr
A good tool to install, articles submitted by community, good examples, nicely formatted 
=== 

### find by name
#### find
`find` - a tool that comes with almost every UNIX(like) system  
_Usage:_ :question: need to find good examples    
[GNU docs](http://www.gnu.org/software/findutils/)  

You can chain commands with find:
_example_
`find . -name "*.docx" -exec rm {} \;`  
reads: find in current directory all items with name ending '.docx' and execute for each one 'rm'  

#### fd
`fd` - even better as knows, which files to ignore etc. and colour codes - grown by the community  

#### locate
looks for **paths** in fs that have a specified substring  
Usage: `locate shell`  

:question::sweaty-smile: What was that about updatedb??

### find by content
#### grep
grep **finds a string** in a specified file, but  
if you use it with the -R swith it will **search an entire directory.**
`grep -R foo ./somesubdir` 

#### ripgrep
fast and more robust than grep
also has a `--files-wihout-match` option thats useful
===

### shell history
#### history

#### <kbd>Ctrl</kbd> + <kbd>R</kbd>

#### fzf
.
===

### navigation

