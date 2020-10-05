Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/command-line/](https://missing.csail.mit.edu/2020/command-line/)  

## TL;DR & personal favourites :icecream:
- `SIGNAL` - shell communicates with programs through signals, _there's more to life than ^C_ - you can not only interrupt, but also pause, abort, continue, report... for a full list `man signal`  
- Stop your jobs from being interrupted - you can use _nohup_ or _import signal_ to your app and change the default behaviour (eg. override SIGINT) - but you can't override SIGKILL  
- You can really make the cmd line environment work for you if you configure it - use _dotfiles_ to change the default settings and _terminal multiplexers_ to use more than one shell window within one emulator window  
- You can control machines remotely through _SSH (secure socket shell)_  
- ...and if you create keys you won't need to authenticate with a password, but _keep your keys safe - add a passphrase to protect the private key & only store the public key on servers_.  
- `htop` - cute process viewer for Linux :sparkles:  

---


## Job Control :video_game:

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
\| tmux has **sessions**  
\| \-\>Sessions have **windows**  
\| \-\-\-\>Windows have **panes**  
(just like in editors you have tabs, which have windows, which have buffers, or in browsers you have sessions, with windows, which have tabs - all called something else, just to make it easier :kissing_smiling_eyes:)   

- control  
**For all commands you need to use a prefix key**:  
<kbd>Ctrl</kbd> + <kbd>B</kbd> - default  
<kbd>Ctrl</kbd> + <kbd>A</kbd> - remapped by many users for convenience  
... followed by a command key. :point_down:  

- prefix + <kbd>?</kbd>  
\- for a **list of bind keys**  

- prefix + <kbd>D</kbd> or `tmux d`  
\-  **dettaches** from the session (like minimising - the process is still running)  
- prefix + <kbd>A</kbd> or `tmux a`  
\- **reattaches** to the session  
- `tmux new -t [session name]`  
\- starts a **new session named** _session name_ (you get a number by default if you don't specify a name)  
- `tmux ls`  
\- **lists** sessions  
- prefix + <kbd>C</kbd> or `tmux c`  
\- create a new **window**  
- prefix + <kbd>P</kbd>  
\- **previous** window  
- prefix + <kbd>N</kbd>  
\- **next** window   
- prefix + <kbd>[window number]</kbd>  
\- jump to a specific window  
You can rename windows after they are open.  
- prefix + <kbd>"</kbd>  
\- pane - **vertical split**   
- prefix + <kbd>%</kbd>  
\- pane - **horizontal split**   
- prefix + <kbd>←↓↑→</kbd>  
\- **move** between he panes   
- prefix + <kbd>Z</kbd>  
\- **zoom** - use once to zoom in and again to zoom out  

## Dot files :page_facing_up:
DOT FILES - Allow you to configure apps (like your shell) so that they work for you.

#### side note - Aliases
> You can **create** aliases for commands you don't want to keep typing over and over again. To do this in your prompt type `alias alias_name="the command"`  for example `alias gs="git status"` will mean you can just type `gs` next time you want to use git status.  
>
>You can **rename** aliases in the same way - `alias new_alias=old_alias`.  
>You can **check** existing aliases by typing `alias alias_name` - you will see the definition  

### .dotfiles  
Most shell programs have a text based config file - a dotfile.  
For historical reasons names begin with a dot, contain config for all sort.  

Examples?  
- **bash** - .bashrc  
- **vim** - .vimrc  
These files will hold config for these particular applications.  If you want to configure any of the apps you're using just google search their dotfiles.  

How to know what to configure?
There is a lot of repos **on github**, folks configuring their apps and sharing the config - **vet before you get!**  
Use lecture resources.  

Where do dotfiles live?
By default - you need to check per app.  
People usually store all of them in one folder,create Symbolic Links (**symlinks**) to the actual location and put it in the location where the system (or applications looking for other applications) expects the dotfile to be.  
Symbolic Links show as `'Symlink Name' -> '/targets/actual/full/path` when you use `ls -l` - an example in Windows would be _'My Documents' -> /c/Users/%username%/Documents_  

**GNU Stow** is a good link for managing symlinks - you can add them safely. Or you can use **ln** to add them yourself -> `ln -s source_file symlink_fyle`  

## Remote machines :computer:
### SSH overview
- secure shell, allows remote connection to machines  
- you can execute command on the remote machine  
- you can create secure **keys** and [store](#Keys) the same key on client and server, so that they can identify your client when attempting an ssh connection (only store your PUBLIC key on the server, keep the private part of the key private)   
- you can use DNS names or IP addresses to [connect](#Connecting)  

### Connecting   
- You can either type in the password for a user that's authorised to use the remote machine, or use a key  
- `ssh username@remote_address`  
If you use just the **ssh** command alone, the shell emulator will switch to the remote machine emulator (so in your window you will see the remote machine's shell)  

### Keys  
- Use ssh-keygen to generate a key ([docs](https://www.ssh.com/ssh/keygen/))  
- Key has two parts - private and public - the public key can be shared with remote machines  
- Passphrase protect your private key!  
- Key's are stored in dif places on different systems, check documentation  
- You need to copy the key to the server you will be connecting to, you can try to cat & tee it from your local machine to the server or use `ssh-copy-id username@server`  

### Executing commands  
- You can then execute the commands on the server by typing `ssh [connection details] [some_cmd]`  
- or **pipe** to your local machine - `ssh username@remote_address [some_cmd] | [cmd executed locally using the output of some_cmd]`  


### Copying files  
- Instead of `cp` you'll be using `scp`  
\- `scp [localpath] username@server:[remotepath]`  
\- can cause duplicates

- for **multiple files** you can use `rsync`  
\- It's amazing because it detects duplicates  
\- check flags  

- you can also use cat + tee  

### Config  
- Config file let's you specify an alias for a host and all connecion details so you can then just call `ssh alias`  
- Other programs will also use the ssh config if they need to use ssh for their jobs  



