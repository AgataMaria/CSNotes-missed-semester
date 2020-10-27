Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/metaprogramming/](https://missing.csail.mit.edu/2020/metaprogramming/)

## TL;DR & personal favourites :icecream:  
- You can automate and standardise things that your program needs to run - create a build script that will build your program the way you want it  
- You can use [`make`](#`make`-&-Makefile) for simple-to-mid projects to create [_targets_](#Targets) using a recipe from the Makefile  
- `make` just chills if none of the dependencies changed, as there's no point in making the same file again. Efficiency. :tropical_drink:  
- wildcards in Makefile are **%** and to get the value you use **$\***, **$@** is a symbol for the name of the target  
- when specifying a version number in your dependencies go for as low as you can in the 'minor' version (second number in the version number - major.**minor**.patch)  
- GitHub pages is basically letting you CI action that builds your repo as a blog - a good example of CI, gotcha  


---

## Metaprogramming  
All things programming that aren't programming - everything that is important about the _process_ and everything around programming, that isn't writing code. This lecture is about building systems to help with programming - systems for building, testing and managing dependencies.  


### :hammer_and_wrench: Build systems  
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
_make_ is a utility built into most systems - it allows building and maintaining groups of programs (and other types of files) from source code.  
When you run `make` it uses a **Makefile** from the current directory. Surprise, surprise, Makefile has the info for how to build (it's like a recipe)  

- Makefile has information on:  
\- target(s)  
\- dependencies  
\- rules  
  
- The structure is:
``` 
target_a: dependency_1 dependency_2  
		an indented block with commands  
		for combining dependencies into targets  

target_b: dependency_3 dependency_4  
		some more rules (to make target_b)
```

The **indented block tells** _make_ **how** to produce targets out of dependencies  
**NB**. target_b may be target_a's dependency - so targets may rely on the result of making other targets in the same Makefile  
  
- Examples:  
from the lecture notes:
```
paper.pdf: paper.tex plot-data.png
	pdflatex paper.tex

plot-%.png: %.dat plot.py
	./plot.py -i $*.dat -o $@
```

here, % is a wildcard, so the second block **creates** _plot-sth.png_ out of _sth.dat_ (data file) and _plot.py_ (python file),  
and **how** it does that is:  
run _./plot.py_ with _-i_ (specified in this particular python file), to mark what the input is,  
the input is _$*_.dat - a d $* is a variable that gets the value of the wildcard (%), so the result is - sth.dat,  
-o is in the python file, nvm,  
_&@_ means the name of the target so plot-sth.png  

**NB.** `make` will check the dependencies and if they haven't checked - it won't run any rules, but rather output `'targetname' is up to date.`  
Handy.


### :dizzy_face: Managing dependencies
Dependencies may mean programs & packages installed locally, or libraries your progamming language is using, your program will need these to run. You don't have to declare all of them as you can often assume they're there, but it's good to keep in mind.  
For most things there are tools that manage dependencies for you.

Apps, packages and libraries your program depends on can be updates/modified over time, which may change the behaviour of, or break, your application.  By convention, whenever software is released for use by public, it will have a **version number**.  

When adding a dependency you can specify a version number of the app/package/library you're using in your program. However, this will mean you won't get updates, which may be beneficial (but then you don't risk unexpected changes).   

#### Semantic versioning
Semantic versioning is an approach where, as a programmer, you give each of the numbers in the version number of your software a particlar meaning (numbers are separated by dots). There is a convention (major.minor.patch) for when to increment the numbers. In an example of a version number _1.0.8_ - 1 would mean the major, 0 minor and 8 the patch version.  
For changing the version number, use these rules:  
**major** version update - backwards imcompatible change (eg. remove a function or rename it would be an example)  
**minor** version update - you are adding something to your library (still compatible)  
**patch** update - change you made is entirely backwards compatible - externally it's as if nothing has changed  

When adding a dependancy in your app, you can specify that 'it has to be the same major version and at least the same minor version as ...'

It's **BEST** to depend on the major + the lowest minor you can.  

#### Lock files
In dependency management systems **lock file** is a file that lists the exact version you are currently depending on of each dependency.  
- It makes sure you don't accidentally update something  
- Your builds are faster, because unless you have updated the version in your lock file - it will just use what was previously built  
- It gives you a reusable setup. Handy.  

#### Vendoring
Means you're copying the entire thing that you depend on rther than referencing it - so no external changes would affect you, because it's now in your app. This is an extreme approach - you know exactly what you have and you can change it how you want, but you now need to worry about maintaining it and you don't get the benefits (newer releases of the original software won't be available for your users)  

### :ok_hand: Testing & Continuous integration
#### Continuous integration
CI is an umbrella term for 'stuff that runs whenever your code changes' - imagine expanding your build system, so that it's externally available and other people working on the same project are using the same build system (that includes documentation, test suites etc.)  
Like a cloud based build system, an event based action system (a system of rules eg. every time you get a PR the code is style checked) Of course there are some robust CI solutions available, some are free for open-source projects. <- :grey_question: need to look for examples.  

> Side note: **dependabot** automatically checks if there are newer versions of dependencies for you  

#### Testing  
Tests really vary in depth and angle.  
- Unit test  
Tests a single feature - what feature means is up to a project  
- Integration test  
Tests interaction between different subsystems of a program  
- Regression test  
Tests thingss that were broken in the past - if you fixed a bug before you add a test to the test suite to make sure when you're upgrading your app in the future you don't do the same mistake  
- Mock test  
 :grimacing: didn't quite get it <- :grey_question: need to read more
