Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/command-line/](https://missing.csail.mit.edu/2020/command-line/)  

## TL;DR & personal favourites
- `SIGNAL` - shell communicates with programs through signals, **there's more to life than ^C** - you can not only interrupt, but also pause, abort, continue, report... for full list `man signal`  
- You can **import signal** in your app and change the default behaviour (eg. override SIGINT) - but you can't override SIGKILL  




## Job Control

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

### Other notes  
\- Don't rely on SIGKILL - it kills the process, but can leave orphaned processes, it only targets current process  



## Terminal Multiplexers

## Dot files

## Remote machines