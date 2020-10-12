Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/version-control/](https://missing.csail.mit.edu/2020/version-control/)

## TL;DR & personal favourites :icecream:
- **git** is one of the version control systems (vcs). Vcs are tools that helps tracking changes history of files and allow collaboration. Git is popular, robust, mysterious, uuuuh-wesome)  
- With vcs you're basically **taking a snapshot** of a state and attach a **message** to it. You also get an author and time stamp, a unique commit ID and some other details that are useful  
- Git is **distributed**, which makes it powerful  


---

## Git data model. Beautiful - on the inside :swan:  
Yeah, you totally need to get over the interface, but once you understand why it's a beautiful data model, you'll be able to appreciate it and bear the face.  

### What's what  
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
\-----------^-- :blue_heart:  
\- - - - - - - - - - fork based on the 2nd snapshot  
 
 (Yes I realise there is an easier way to represent this).  


 You can separate what you work on and work in parallel to yourself and others - eg. when you want to split the problems and work with one version of your code for feature development and one for bug fixing. You don't want to handle changes in both, as it's too much and can get messy easily. However, since your starting point is the same, git let's you remember the point in history / version of your source code where you forked off (tee hee) and went to do your things, and will intelligently let you remember and compare all future changes, rather than having to have full copies of your code to work on, and then try to piece it back together yourself. Once you've worked on :heart: and :blue_heart: and you're happy to put them together, you can **merge them**.  
 This creates a new snapshot :purple_heart: that contains all changes.  
  
**merge conflicts** - of course if you worked on something in one branch that affects the other and merging may create a problem - git will warn you about a merge conflict and let you figure out how you want to piece is together.  

