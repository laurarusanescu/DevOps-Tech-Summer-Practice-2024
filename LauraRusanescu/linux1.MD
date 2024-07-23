____ls -l____________

Linux has differente timestamps, three, to be exact.
 -- atime - the last time when file was accessed  
 -- mtime - last modification time. By modification we mean change in the file content.  
 -- ctime - last metadata modification time. We mean here - permissions change, location of the file, etc.  

 -s: Shows allocated space.  
-S: Sorts by size.  
-h: Human-readable sizes (powers of 1024).  
--si: Human-readable sizes (powers of 1000, less common).  

_______dir____________

Now we used some arguments(for rmdir):   
-r means go recursively through directories (and treat everything as file)  
-f - force. Another words, do not ask, assume the user knows what he is doing.  
