
Official Notes: [https://missing.csail.mit.edu/2020/shell-tools/](https://missing.csail.mit.edu/2020/shell-tools/)

## Intro

_From Wiki:_  
> A **shell script** is a computer program designed to be **run by the Unix shell**, a command-line *interpreter*. The **various dialects of shell scripts are considered to be scripting languages**. ... Related programs such as shells based on Python, Ruby, C, Java, Perl, Pascal, Rexx &c in various forms are also widely available.

So in shell script you can use variables, for-loops etc.

### Variables
**syntax**  
To _create vars_ you just declare and assign them -->
`foo=bar` # **DO NOT USE SPACES**   

To _access foo_ you use a `$` --> `$foo`   

For **string interpolation/substitution** you need to use double quotes -->  
`"Foo has a value of $foo"` CORRECT  
`'Foo has a value of $foo'` WRONG  

### Functions
**syntax**  
To declare a function use '**fname** **()** **{** _functionbody_ **}**'  
To grab arguments from the command line instead of argv use `$numer` to interpolate to your function body  
_example_
	mcd () {
		mkdir -p "$1"
		cd "$1" 
	}

## Using shell script
?? whats vim?  

### Argument shorthands**  
**$0** - name of the script   
**$_** - last argument of the previous cmd  
**$#** - number of arguments  
**$@** - all arguments  
**!!** - last cmd  
**$?** - error from the previous cmd  
**$$** - cmd process id  
**{range}** - eg. `touch foo{1,9}` will touch foo1, foo2, foo3... foo9.  
**?** - placeholder for a single number eg. `dir?` could equate to anything in range dir0-dir0, dir10+ would be ignored  


### Logic**
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
?? dont get it :D

**-ne**  
not equals

