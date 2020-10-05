Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/command-line/](https://missing.csail.mit.edu/2020/command-line/)  

## TL;DR & personal favourites
- `SIGNAL` - shell communicates with programs through signals, _there's more to life than ^C_ - you can not only interrupt, but also pause, abort, continue, report... for a full list `man signal`  
- Stop your jobs from being interrupted - you can use _nohup_ or _import signal_ to your app and change the default behaviour (eg. override SIGINT) - but you can't override SIGKILL  




## Job Control

### Hotkeys & cmds

- <kbd>Ctrl</kbd> + <kbd>C</kbd>  
\- **cancels** any job you're running - interrupts!  
\- it works because shell sends a signal called **SIGINT** (SIGnal INTerrupt) that tells the program to stop itself  
\- there are many **other signals** available - you can lookup using `man signal` or go to [man7.org](https://man7.org/linux/man-pages/man7/signal.7.html)  

- <kbd>Ctrl</kbd> + <kbd>\\</kbd>  
\- **quits** the execution of the program (**SIGQUIT**)  

- <kbd>Ctrl</kbd> + <kbd>Z</kbd>  
\- **suspends** the program (**SIGSTOP**) - use _bg_ to resume  

- **&** -  `[cmd] &`   
\- **runs** the command **in the background**  
\- this gives you access to shell & starts the process (otherwise if the command is waiting for input from the shell you can't run any other commands)

- `jobs`  
\- **lists jobs** currently running

- **bg** - `bg %n`  
\- **resumes** a suspended job - %n is a job number (use `jobs` to get the number)  
 
- `kill %n`  
\- there are many ways to kill - you can **use a modifier** to specify **how you want to kill** the job  
\- examples:  
`kill -STOP %n` - _pauses_ the process (%n is a job number)  
`kill -HUP %n` - _hungs up_ the job - except for jobs that use _nohup_  


### Other notes  
\- Don't rely on SIGKILL - it kills the process, but can leave orphaned processes, it only targets current process  


## Terminal Multiplexers
### Say what now?
Sounds futuristic, I know. They exists so that you **don't need to use multiple terminal windows**, by creating an **environment** for you to work in where you can have multiple sessions etc  
`tmux` is an example of a terminal multiplexer (recommended in the course)  
> tmux is a terminal multiplexer for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window.  

### tmux - basics
- tutorial
[recommended tutorial](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)  

- nesting  
tmux has **sessions**  
\-\>Sessions have **windows**  
\-\-\-\>Windows have **panes**  
(just like in editors you have tabs, which have windows, which have buffers, or in browsers you have sessions, with windows and tabs - just to make it easier)   

- control  
For all commands you need to use a **prefix key**:
<kbd>Ctrl</kbd> + <kbd>B</kbd> - default  
<kbd>Ctrl</kbd> + <kbd>A</kbd> - remapped by many users for convenience  
... followed by a command key. :point_down:  

prefix + <kbd>?</kbd>  
\- for a **list of bind keys**  

`tmux d` or prefix + <kbd>D</kbd>  
\-  **dettaches** from the session (like minimising - the process is still running)  
`tmux a` or prefix + <kbd>A</kbd>  
\- **reattaches** to the session  
`tmux new -t [session name]`  
\- starts a **new session named** _session name_ (you get a number by default if you don't specify a name)  
`tmux ls`  
\- **lists** sessions  
`tmux c` or prefix + <kbd>C</kbd>  
\- create a new **window**  
prefix + <kbd>P</kbd>  
\- **previous** window  
prefix + <kbd>N</kbd>  
\- **next** window   
prefix + <kbd>[windows number]</kbd>  
\- jump to a specific window  
You can rename windows after they are open.  
prefix + <kbd>"</kbd>  
\- pane - **vertical split**   
prefix + <kbd>%</kbd>  
\- pane - **horizontal split**   
prefix + <kbd>←↓↑→</kbd>  
\- **move** between he panes   
prefix + <kbd>Z</kbd>  
\- **zoom** - use once to zoom in and again to zoom out  

## Dot files
DOT FILES - Allow you to configure your shell so that it works for you.

#### side note - Aliases
You can **create** aliases for commands you don't want to keep typing over and over again. To do this in your prompt type `alias alias_name="the command"`  for example `alias gs="git status"` will mean you can just type `gs` next time you want to use git status.  

You can **rename** aliases in the same way - `alias new_alias=old_alias`.  
You can **check** existing aliases by typing `alias alias_name` - you will see the definition  

### .dotfiles  
Most shell programs have a text based config file - a dotfile.  
For historical reasons names begin with a dot, contain config for all sort.  

**Programs + dotfiles**  
- **bash** - .bashrc


## Remote machines