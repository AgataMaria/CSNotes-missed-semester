Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/metaprogramming/](https://missing.csail.mit.edu/2020/metaprogramming/)

## TL;DR & personal favourites :icecream:  
- You can use [`make`](#`make`-&-Makefile) for simple-to-mid projects to create [_targets_](#Targets) using the recipe from a Makefile  
- `make` just chills if none of the dependencies changed, as there's no point in making the same file again  


---

## Metaprogramming  
All things programming that aren't programming - everything that is important about the _process_ and everything around programming, that isn't writing code. This lecture is about building systems to help with programming - systems for building, testing and managing dependencies.  


### Build systems  
Combining / encoding commands & rules that you would run to carry out a task into a tool that does the job for you. If you typically carry out a sequence of tasks / run commands that allow you to build something, you can automate this.  
You would teach the tool about [_dependencies_](#Dependencies) between [_targets_](#Targets). There are many tools ready to use, built for different purposes (eg. _npm_ has a tool built in for tracking of dependencies), but the idea is the same and they all have some things in commong, like:  

#### - Targets  
Things that you want to build - can be 'myfile.pdf', but can also be _run the test suite_ or _build a binary for this program_  
Every tool needs targets to operate on  

#### - Dependencies  
These are things that need to be built for the target to be built  
Check out [Managing dependencies](#Managing-dependencies) for more info  

#### - Rules  
Sequences of commands that allow for the target to be built out of depenencies - think of it as all steps (commands) needed to combine the elements (dependencies) into the final result (target)  
How you encode these rules changes depending on the tool - in the lecture they use [make](#make)  

#### `make` & Makefile
A utility built into most systems - allows building and maintaining groups of programs (and other types of files) from source code.  
When you run `make` it uses a **Makefile** from the current directory  
- Makefile structure:  
Makefile has information on:  
\- target(s)  
\- dependencies  
\- rules  
  
The structure is:
``` 
target_a: dependency_1 dependency_2  
		an indented block with commands  
		for combining dependencies into targets  

target_b: dependency_1 dependency_2  
		some more rules
```

The indented block tells _make_ how to produce targets out of dependencies  
**NB**. target_b may be target_a's dependency - so target_a may rely on the result of making target_b  
  
- Examples:  
from the lecture notes:
```
paper.pdf: paper.tex plot-data.png
	pdflatex paper.tex

plot-%.png: %.dat plot.py
	./plot.py -i $*.dat -o $@
```

here, % is a wildcard, so the second block **creates** _plot-sth.png_ out of _sth.dat_ (data file) and _plot.py_ (python file), and **how** it does that is: run _./plot.py_ with the _-i_ specified in the python file,to mark what the input is, the input is _$*_.dat a d $* is a variable that gets the value of the wildcard (%), so - sth.dat, -o is in the python file, _&@_ means the name of the target so plot-sth.png  

**NB.** `make` will check the dependencies and if they haven't checked - it won't run any rules, but rather output `'targetname' is up to date.`  
Handy.


### Managing dependencies
Dependencies may mean programs & packages installed locally, or libraries your progamming language is using, your program will need these to run. You don't have to declare all of them as you can often assume they're there, but it's good to keep in mind.  
For most things there are tools that manage dependencies for you.

Apps, packages and libraries your program depends on can be updates/modified over time, which may change the behaviour of, or break, your application.  By convention, whenever software is released for use by public, it will have a **version number**.  

When adding a dependency you can specify a version number of the app/package/library you're using in your program. However, this will mean you won't get updates, which may be beneficial (but then you don't risk unexpected changes).  

#### Semantic versioning
Semantic versioning is an approach where, as a programmer, you give each of the numbers in the version number of your software a particlar meaning (numbers are separated by dots). There is a convention on when to increment the numbers - in a version number _1.0.8_ - 1 would be the major, 0 minor and 8 a patch version.  
**major** version update - backwards imcompatible change (remove a function or rename it would be an example)  
**minor** version update - you are adding something to your library  
**patch** update - change you made is entirely backwards compatible - externally it's as if nothing has changed  
When adding a dependancy you can say 'it has to be the same major version and at leasdt the same minor version'  

It's good to depend on the major + the lowest minor you can.  

