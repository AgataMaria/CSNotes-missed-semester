Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/debugging-profiling/](https://missing.csail.mit.edu/2020/debugging-profiling/)

## TL;DR & personal favourites :icecream:

---

## Debugging
There are different approaches to debugging, each with their own perks.

### `printf` debugging (or print, console.log etc. in other languages)  
You put a print statement somewhere to check eg. your variables’ value, or a note to confirm that some command has been executed.  It’s fast and useful for fixing things on the spot.
- adding print statements directly in the code, reading from stdout
- fast and easy
- well placed print statements go a long way
- printf debugging is an essential skill

### Logging
Incorporating logging into your program is a good practice - you can simply print to stdout when something happens or print to a log file, a remote server etc. There are tools that help with logging and reading logs.
- it’s about logging events as they happen
- you can log to different sources - eg. files, stdout, remote sockets, remote servers
- allows you to categorise log messages - for different severity levels eg. INFO, DEBUG, WARN, ERROR etc.
- there are tools that make writing logs neater (come with formatters) and reading easier (you can filter logs quickly)
- as apps get more complex logs are necessary - if something goes wrong you already should have all info you need from he logs

#### Side note:  
**Colourising logs to stdout** - you can colour code the log messages to make them easier to read.  
NERD OUT HERE → A lot of programs are using [ANSI escape codes](https://en.wikipedia.org/wiki/ANSI_escape_code) to format the messages.  
Check the color.sh for what colours are suppored by the console.

#### Side note #2:  
**Client side erroring** - of course you need to notify user in some way if something unexpected happens, but for godness sake have your own logs (there are different approaches to client side behaviour (RABBIT HOLE HERE → for web see also [progressive enhancement](https://en.wikipedia.org/wiki/Progressive_enhancement#:~:text=Progressive%20enhancement%20is%20a%20strategy,Internet%20connection%20of%20the%20user.))  

### System logs  
A lot of modern systems will allow this and a lot of modern applications will log to a unified system log.  
- in Linux logs are usually kept in - `/var/logs`  
- apps can log to their own log + unified system log  
- system logs are not in plain text so you need a tool to read them - `journalctl` is your friend :) (checkout the man page)  

### Debuggers
There are different debuggers, the lecture discussed & demoed using IPython, but most languages have some sort of a debugger for them. IDEs commonly come with debuggers (and so do modern browsers).  IPython probably won’t be massively useful to me so not going into details, but for all debuggers the principles are the same - you have:  
- breakpoints  
you can set breakpoints in your code - the program will pause executing at each breakpoint and you can inspect the state of the app at that time  
- step  
you can ‘step in’ to the line at which you stopped - meaning you will execute this particular line and halt again - you can step through your program line by line  
- next  
continue executing until the next breakpoint  
- printing  
there is usually a facility to look up the values of the variables / state of the app as you step through your program

### Debugging from system calls  
Your system is aware of everything that the apps are doing - as they always interact with the system in some ways.  
Apps need to do **system calls** sometimes to access some resources, read/write to a file, open a network connection etc. so by checking system calls you can see what happened.  
- `strace <program name>`  
let’s you view all of the system calls made by the program - try `strace ls <dir name>` to see the output  
- `dtruss <program name>`  
same as _strace_
- `tcpdump`  
for network analysis  
- Wireshark  
an application with a friendly GUI for network analysis  


### Static analysis tools  
In the lecture they discussed pyflakes & mypy - need to check equivalents for non-python apps  
Also `writegood` which analises your (English) writing, checks spelling, and gives stylistic tips. Yes, please!  

From lecture notes:  
> For web development, the Chrome/Firefox developer tools are quite handy. They feature a large number of tools, including:  
> Source code - Inspect the HTML/CSS/JS source code of any website.  
> Live HTML, CSS, JS modification - Change the website content, styles and behavior to test (you can see for yourself that website screenshots are not valid proofs).  
> Javascript shell - Execute commands in the JS REPL.  
> Network - Analyze the requests timeline.  
> Storage - Look into the Cookies and local application storage.  

## Profiling  
