Original Notes: [https://missing.csail.mit.edu/2020/course-shell/](https://missing.csail.mit.edu/2020/course-shell/)

### shell - the basics

#### What is shell?
Basic tool to interact with your computer - allows you full access to it's resources, it's a textual interface, GUI has it's limiations.

Most platforms provide their own shells - examples:
Windows - PowerShell
Linux - bash (Born Again SHell)
MacOS - also bash (used by the terminal app)

##### EXTRA
_Useful:_ https://explainshell.com/


#### Getting started:
It will probably already be installed on your platform, you can Google it if you don't know how to access it.

When you open shell you see a prompt, that's customisable, but usually it's a combination of  
[username@machinename currentpath]$  
(basic form)  

On the shell prompt you get to write commands - usually this is a name of an app + arguments.  
_examples:_  

**date**  
\- runs the date app, with no arguments displays current date and time.

**echo [arg]**  
\- prints out the argument - for example:
	echo 'Jake the Dog'
	Jake the Dog

**NB.** When passing strings containt them in **single or double quotes**, or escape the white space with '\\'. (doesn't work in PowerShell??)


#### How shell works:
Shell knows, which program to execute when we type 'date' or 'echo', because the path value is stored in a variable on your file system (*environment variables* - examples would be your homefolder, some app paths etc.).  
Shell script is basically a programming language, you execute commands, but can also use while loops, for loops, define functions, have variables etc.  

Environment variables do not have to be defined whenever you start your shell - they are already set.  

_Useful:_ which [app name]  
\- will tell you which path it would use if you wanted to run it.


### paths 
Full locations of a resource in your file system. In Linux and MacOS everything is mounted to one root name space and then subdirectories are separated by '/'.  
In Windows a '\' is used to separate directories and every partition has it's own root that has a letter assigned to it (eg. C:\ D:\).  
##### EXTRA 
That's because '/' had a different use in MS-DOS - more info [here](https://www.howtogeek.com/181774/why-windows-uses-backslashes-and-everything-else-uses-forward-slashes/)

**Absolute paths** - 'full' paths - eg. /use/bin/ | C:/Users/%username%/Documents  
**Relative paths** - path's relative to your current location - eg. ./newdir | './New Folder'.  
(special directories - '.' and '..' - current and parent directory).  

Generally any app we use wil by default **RUN IN THE CURRENT DIRECTORY (!) unless we give it specific arguments.**  

### commands - basics (flags / options (switches) / help) 
Most programs take optional arguments that change their behaviour - options and flags usually start with - or -- (also / in Windows - eg. shutdown /r).
Anything that takes a value is an option, anything that doesn't take a value is a flag.
_Examples:_ _ls -l_ # you can use just ls, but ls -l give you a more detailed view

**--help**  
Most programs also implement --help (also -h or /? in Windows), if you add this to the program name it will print out some information about the program - typically usage information.  
How to read: ls [OPTION]... [FILE]...  # here **'...'** means 1 or more, **'[ ]'** means optional.  
Usage: _ls --help_

**man**  
A program that takes a name of another program as an argument and gives you it's manual page. It's usually easier to read and sometimes has more info than --help.  
Usage: _man ls_

**NB.** This is a program so to go back you need to quit it - by hitting 'q'


### commands - navigating file system
**pwd**  
\- to find out where you currently are - use commant pwd (print working directory).

**cd**  
\- change current directory,
\- can combine with .. and . to jump up to a parent a directory or to a subdirectory of a current directory,
\- can also combine with '-' to go 'back'.

	cd ./subdir
	cd ..
	cd ../.. # would take you two directories up
	cd - # takes you to the directory you were previously in (not in PowerShell)

_Useful:_ hitting <kbd>TAB</kbd> after typing the beginning of a directory/file name will auto-complete, choosing the first available option - hitting tab again and again will go through all options matching what you already typed.  

_Useful:_ **~** always means home directory, you can use it as an argument for cd, ls etc. For example '~/documents' would go to a documents folder in your home directory.

**ls**  
\- list everything in the current directory - files and directories (including '.' and '..' directories).
\- can take a path (absolute or relative) as an argument, so running 'ls ./subdir' will list all files and dirs in the subdirectory. 
(ls with -l gives you a detailed view - more info below, check ls --help for full usage)

**mv**  
\- let's you move the file... and also rename it :D
mv [previous path] \[new path]  <-- if the last bit changes you're effectively renaming the file

**cp**  
\- let's you copy a file

**rm**  
\- let's you remove a file (but not a dir!) - you can use a dir path with the -r flag to do this recursively (to empty a folder)

**rmdir**  
\- let's you remove a dir... but only if it's empty

**mkdir**  
\- let's you create a new directory

**ls -l**  
\- Gives you a 'long listing format' - a lot more information that ls without the flag
How to read:  

	-rw-r--r-- 1 jon jon     102 Dec 5 14:06 404.html  
	drwxr-xr-x 1 jon jon	6692 Dec 5 14:06 _data  

	first letter - d would mean a directory
	(then we have 3 sets of 3 characters - permissions for user owner, group owner, everyone else)
	rw- # means read write
	r-- # read permissions
	rwx # read, write, execute (works funny for dirs see below)

**NB.** For directories permissions work like this - Read means list contents, Write means rename, create or remove files within this directory, Execute allows you to ENTER this directory - you need permission on this directory and all it's parent directories.


### commands - IO, chaining and writing to files
You can chain commands by using streams - the primary streams are input steam and output stream (standard input output stdio).
By default input is from terminal and output to terminal - but you can use shell to redirect these to/from a different source.

**< - input**  
\- Use '< [source] to change the input stream  
_< infile_ would mean the input for your command would be set to infile

**> - output**  
\- Use '< [source] to change the output stream  
_< outfile_ would mean the ouyput of your command would be set to outfile

**cat**  
\- prints contents of a file - if you combine with < & > you can essentially copy it's contents to another file  
_cat < myoldfile > mynewfile_

**>>**  
\- appends to a file

**| (pipe)**   
\- make whatever you do on the left | output to the right
somecommand | myfile.txt

**tail**  
\- grabs the last lines of its input (lets you read the end of the file)

**combining it all**  
_somecommand | cat > result.txt_  
on the left we do some operations | on the right we use cat to write to a file named result.txt - what we're writing is the result of the command from the left


### Linux and MacOS - root user
Special user - like Administrator (built-in) in Windows.  
Has a user number 0.  
**root user can do whatever it wants on your system.**  
**#** youll see a pound symbol if you're running as root.  
_# somecommand_ syntax basically means run this command as root.  

**sudo**  
'Do as SU (superuser)' - a program that let's you elevate access so you can use a a user with sudo rights instead of root.

_sudo somecommand_ you just need to preceed whatever you want to do with sudo

**NB.** Permission denied example - TL;DR, shell does not have elevated permissions itself, use root. 
In Linux 'everything is a file', so is screen brightness. We tried _sudo echo 500 > brightness_, but this wouldn't work as it was the shell that was trying to open the brightness file, not sudo.
