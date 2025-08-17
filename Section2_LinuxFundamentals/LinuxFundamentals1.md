## Linux Fundamentals 1

### Basic Commands
- `ls` → lists files in the current directory  
- `cd` → change directory  
- `whoami` → shows the current logged-in user  
- `cat <file>` → prints contents of a file  

---

### The find Command 🔍
Used to search for files in directories.  

- Find a specific file by name:  
  `find -name passwords.txt`  
  Output: `./folder1/passwords.txt`  

- Find all `.txt` files (using * wildcard):  
  `find -name "*.txt"`  
  Output:  
  `./folder1/passwords.txt`  
  `./Documents/todo.txt`  

---

### The grep Command
Used to search inside files for matching patterns.  

- Count how many lines are in a file:  
  `wc -l access.log`  
  Output: `244 access.log`  

- Search access logs for a specific IP:  
  `grep "81.143.211.90" access.log`  
  Output:  
  `81.143.211.90 - - [25/Mar/2021:11:17 +0000] "GET / HTTP/1.1" 200 417 "-" "Mozilla/5.0 (Linux; Android 7.0; Moto G(4))"`  

---

### Shell Operators 🐚
- `&` → Run a process in the background  
  Example: `ping google.com &`  

- `&&` → Run the next command only if the previous one succeeds  
  Example: `mkdir test && cd test`  

- `>` → Redirect output to a file (overwrite)  
  Example: `ls > files.txt`  

- `>>` → Append output to a file  
  Example: `echo "new line" >> files.txt`  
