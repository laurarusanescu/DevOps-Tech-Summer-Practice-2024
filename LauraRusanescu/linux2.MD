__________ps ____________
(process status)  
PID - quite obvious, this is the process id.  
TTY - Terminal associated with the process. Here is a very detailed reading about TTY.  
TIME - total time of CPU usage  
CMD - the command which is running.  
Statuses  
D - uninterruptible sleep (usually IO)  
I - Idle kernel thread  
R - running or runnable (on run queue)  
S - interruptible sleep (waiting for an event to complete)  
T - stopped by job control signal  
t - stopped by debugger during the tracing  
X - dead (should never be seen)  
Z - defunct ("zombie") process, terminated but not reaped by its parent  
  
(second letter)    
< - high-priority (not nice to other users)  
N - low-priority (nice to other users)  
s - is a session leader  
l - is multi-threaded  
+ - is in the foreground process group  
  
ps -ef: Detailed list of all processes.  
ps aux: Comprehensive process information.  
ps aux --forest: Tree-like process hierarchy.  
pstree: Visualizes process tree.  
Filtering Options:  
ps -f -u username: Processes by user.  
ps -f -C command: Processes by command name.  
ps -f -p PID: Process by PID.  
ps -f --ppid PID: Processes by parent PID.  
ps -f -p PID1,PID2,PID3: Multiple PIDs.  

_________Alias____  
alias al="ls -l"  
grep "alias" .bashrc OR echo "alias lh='ls -alh'" >> .bashrc(make aliases permanent)
