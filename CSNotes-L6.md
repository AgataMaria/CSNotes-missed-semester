Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/version-control/](https://missing.csail.mit.edu/2020/version-control/)

## TL;DR & personal favourites :icecream:
- **git** is one of the version control systems (vcs). Vcs are tools that helps tracking changes history of files and allow collaboration. Git is popular, robust, mysterious, uuuuh-wesome)  
- With vcs you're basically **taking a snapshot** of a state of your directory / codebase and attach a **message** to it. You also get an author and time stamp, a unique commit ID and some other details that are useful  
- References to all snapshots (commits) and objects (folders | files | commits) are stored in a **repository** in a .git file.  
- What lives in a .git file is:  
`HEAD config description hooks info objects refs`  
_(lighbulb moment)_  
- `git log --all --graph --decorate` is a hero  
- apart from keeping a local repo, you can create one on a remote server - like a github.com type service - so you can share it with others or have a version you can connect to from other devices - use `git remote` to add info about a remote repo to your local repo so git knows where to push changes to  
- `git checkout` let's you switch to any previous node - referenced by hash (long hex string) - or the most recent node on the chosen branch - referenced by branch name  
- **master** is special - by design  
- **origin** is special - by convention  
- `git commit --dry-run` is excellent for those with commitment issues




---

## Git data model. Beautiful - on the inside :swan:  
Yeah, you totally need to get over the interface, but once you understand why it's a beautiful data model, you'll be able to appreciate it and bear the face.  

### What's what - part 1  
Git **data model** represents a history of some files and folders _as a series of snapshots_. Let's take the files and folders first:  
- **folder** - is called a **tree**  
- **file** - is called a **blob**  
- Trees can contain other trees and blobs (obvs)  
- **root** is the top of the model - like a directory that contains all trees  

### Where do snapshots live then?  
So snapshots are like bubbles that **CONTAIN ALL** of the trees and blobs **in their current state** (at the time of snapshot).  
Every snapshot points to a previous snapshot  
:heart: <---- :heart: <---- :heart:  
firsts - - - next - - - most recent  

It doesn't have to be this linear!  

firsts - - - next - - - most recent  
:heart: <---- :heart: <---- :heart:  
\-----------^-- :blue_heart:------  
\- - - - - - - - - - fork based on the 2nd snapshot  
 
 (Yes I realise there is an easier way to represent this).  


 You can separate what you work on and work in parallel to yourself and others - eg. when you want to split the problems and work with one version of your code for feature development and one for bug fixing. You don't want to handle changes in both, as it's too much and can get messy easily. However, since your starting point is the same, git let's you remember the point in history / version of your source code where you forked off (tee hee) and went to do your things, and will intelligently let you remember and compare all future changes, rather than having to have full copies of your code to work on, and then try to piece it back together yourself. Once you've worked on :heart: and :blue_heart: and you're happy to put them together, you can **merge them**.  
 This creates a new snapshot :purple_heart: that contains all changes.  
  
**merge conflicts** - of course if you worked on something in one branch that affects the other and merging may create a problem - git will warn you about a merge conflict and let you figure out how you want to piece is together.  

### What's what - continued  

From the original notes:
```
// a file is a bunch of bytes
type blob = array<byte>

// a directory contains named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metadata, and the top-level tree
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
// git is a collection of objects of any type
type objects = blob | tree | commit

// this is git
objects = map<string, objects>
```

so _blob_ is the substance, _tree_ is a map / something describing the addresses and structure of other trees (map) and blobs (substance), a _commit_ is a structure, an object that has information about the whole structure (by reference) + metadata like author, message and a reference to previous commits.  If you think of an _object_ as an instance of any one of those types, _git_ is a _collection of mapped out objects_, it's the data, the description of a structure, details from the commits and a description / map of how it all fits together, all stored in a form that git can understand and operate on, so if you say `git status`, git knows how to read the information it contains to give you (own interpretation never to be cited to another human being ever again ever).  
  
>Interesting: Git stores all these objects as a SHA-1 hash of snapshots  
>SHA-1 is a cryptographic hash function which takes an input and produces a 160-bit (20-byte) hash value known as a message digest – typically rendered as a hexadecimal number, 40 digits long.  

---

## Git - how to use :point_down:

### Commands & Concepts  
#### Basic

- `git init`  
Initialises the **repository** (creates a .git file, which will contain **objects, references** other data git needs)  
here's what's inside - `HEAD config description hooks info objects refs`  
  
- Staging - what's tracked?  
Git let's you choose which files to actually track, so when you take a snapshot it does not need to be a snapshot of the entire dir. You can either add individual files / dirs (blobs / trees) or you can add the whole dir, but have a .gitignore file, which is a _dotfile_ with info on what to ignore during the staging area  
  
- `git add  <file> | <dir`  
Stages files to include in the next snapshot. `git add foo.txt` will stage _foo.txt_ and `git add .` will add the entire current directory. `git add :/` would add the entire directory root down.  
  
- `git commit`  
Read as 'take a snapshot' - it will create a hash of all trees and blobs staged when you did _git add_. Makes sense now? (it gives you a hash you can reference this particular commit by, if you cat-file -p _hash_ you can see what it contains :))  
  
- `git commit --dry-run`  
 from Git documentation:
 > Do not create a commit, but show a list of paths that are to be committed, paths with local changes that will be left uncommitted and paths that are untracked.
  
- `git status`  
Shows the history of changes   
  
- `git log`  
Shows the history in a nicer format - when you use `git log --all --graph --decorate`  
Displays a history of commits and where HEAD is and where all other branches are (which point in history / version of code have they been updated to - through add & commit)  
It's a program so you have to use `q` to quit :)  
  
- HEAD  
Current node - the current branch and commit on that branch  
When you check log you can see which node is the head node currently  
  
- `git checkout` (going back)  
Let' you switch nodes - you can checkout to the latest version of a chosen branch (like master or some other branch references by name) or go back to a previous commit  
To go back to a previous commit - you need to know the commit hash (long hex string) - you can look it up using git log.
then use `git checkout <commithash>`  
**This will move the HEAD node and also change the contents of your current directory - so check before you force it.**  

**To iscard most recent changes** and going from tthe file version in your current directory to the one in HEAD use `git checkout <file>`  
_See also git checkout in [Branching & Merging](#Branching-&-Merging)  
  
- `git diff <file>`  
Shows you the changes made to a <file> - it can take different arguments, so you can compare different snapshots of the same file,  so `git diff filename.foo` will show you the most recent changes made in HEAD for filename.foo.

#### Branching & Merging  

- `git branch`  
Displays branches  
- `git branch <branch name>`  
Creates a new branch called <branch name> (doesn't switch to this branch!)  
- `git checkout <branch name>  `
Switches to <branch name>  
- `git checkout -b <branch name>`  
Equivalent of `git branch <name>; git checkout <name>`  
Creates & Switches to the new branch  
- `git merge <commit hash>`  
- `git mergetool`  
- `git rebase`  
 
#### Remote repo  











-