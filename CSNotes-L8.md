Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/metaprogramming/](https://missing.csail.mit.edu/2020/metaprogramming/)

## TL;DR & personal favourites :icecream:  

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
  
- Examples:  