Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/command-line/](https://missing.csail.mit.edu/2020/command-line/)  

## TL;DR & personal favourites
- `SIGNAL` - shell communicates with programs through signals, **there's more to life than ^C** - you can not only interrupt, but also pause, abort, continue, report... for full list `man signal`  
- Stop your jobs from being interrupted - ou can _use `nohup`_ or _import signal_ in your app and change the default behaviour (eg. override SIGINT) - but you can't override SIGKILL  




## Job Control

### Hotkeys & cmds

- <kbd>Ctrl</kbd> + <kbd>C</kbd>  
\- **cancels** any job you're running - interrupts!  
\- it works because shell sends a signal called **SIGINT** (SIGnal INTerrupt) that tells the program to stop itself  
\- there are many **other signals** available - you can lookup using `man signal` or go to [man7.org](https://man7.org/linux/man-pages/man7/signal.7.html)  

- <kbd>Ctrl</kbd> + <kbd>\\</kbd>  
\- **quits** the execution of the program (**SIGQUIT**)  

- <kbd>Ctrl</kbd> + <kbd>Z</kbd>  
\- **suspends** the program (**SIGSTOP**)  

- **&** -  `[cmd] &`   
\- **runs** the command **in the background**  
\- this gives you access to shell & starts the process (otherwise if the command is waiting for input from the shell you can't run any other commands)

- `jobs`  
\- **lists jobs** currently running

- `bg %n`  
\- **resumes** a suspended job - %n is a job number (use `jobs` to get the number)  
 
- `kill`  
\- there are many ways to kill - you can **use a modifier** to specify **how you want to kill** the job  
\- examples:  
`kill -STOP %n` - _pauses_ the process (%n is a job number)  
`kill -HUP %n` - _hungs up_ the job - except for jobs that use _nohup_  


### Other notes  
\- Don't rely on SIGKILL - it kills the process, but can leave orphaned processes, it only targets current process  


## Terminal Multiplexers
### Say waht now?
It exists so that you **don't need to use multiple terminal windows** by creating an **environment** for you to work in  
`tmux` is an example (recommended in the course)  
> tmux is a terminal multiplexer for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window.  

### tmux - basics
- nesting :nesting_dolls:
tmux has **sessions**  
\	Sessions have **windows**  
\		Windows have **panes**  
(just like in editors you have tabs, which have windows, which have buffers - nesting!  

- control
<kbd>Ctrl</kbd> + <kbd>B</kbd> + <kbd>D</kbd>  - default  
<kbd>Ctrl</kbd> + <kbd>A</kbd> + <kbd>D</kbd>  - remapped by many users  
\-  dettaches from the session (like minimising - the process is still running)  
  
`tmux a`  
\- reattaches to the session  
  
`tmux new -t [session name]`  
\- starts a new session names [session name] \(you get a number by default)  

`tmux ls` - lists sessions  
  


## Dot files

## Remote machines