## Windows Command Line

📦 Tools vs Shells (clarity on "what runs where?")

| Tool                 | Terminal Emulator (the window) | Shell Inside (the interpreter)   | Default on Windows? |
| -------------------- | ------------------------------ | -------------------------------- | -------------------- |
| **CMD**              | Yes                            | `cmd.exe`                        | ✅                   |
| **PowerShell**       | Yes                            | `powershell.exe`                 | ✅                   |
| **Windows Terminal** | Yes (modern multiplexer/host)  | Any (CMD, PowerShell, Bash, WSL) | ✅                   |
| **Git Bash**         | Yes (bundled with Git)         | `bash` (via MSYS2/MinGW layer)   | ❌ (needs install)   |
| **Linux Terminal**   | Yes (e.g., GNOME Terminal)     | `bash` / `zsh` / others          | ✅ (on Linux)        |


## 🔹 System Info
```cmd
ver                  :: Windows version
systeminfo           :: Detailed system info
````

## 🔹 Network

```cmd
ipconfig             :: Show network config
ipconfig /all        :: Show full details
ping google.com      :: Test connectivity
tracert google.com   :: Trace route
nslookup example.com :: DNS lookup (default server)
nslookup example.com 8.8.8.8 :: DNS lookup using specific server
netstat              :: Active connections
netstat -abon        :: With process IDs and names
```

## 🔹 File & Directory Management

```cmd
cd                   :: Show current dir
cd C:\Users          :: Change dir

dir                  :: List files
dir /a               :: Show hidden files
dir /s               :: Recursive listing

tree                 :: Show folder structure

mkdir Projects       :: Create folder
rmdir Projects       :: Remove empty folder

type notes.txt       :: Display file contents
more notes.txt       :: Paginate large file

copy report.txt D:\Backup      :: Copy file
copy *.txt D:\Backup           :: Copy all txt files
move report.txt D:\Docs        :: Move file
del oldfile.txt                :: Delete file
erase oldfile.txt              :: Delete file (same as del)
```

## 🔹 Task & Process Management

```cmd
tasklist                            :: Show running processes
tasklist /FI "imagename eq notepad.exe" :: Filter for Notepad
taskkill /PID 1234                  :: Kill process by PID
taskkill /IM notepad.exe            :: Kill process by name
```

## 🔹 Shutdown / Restart

```cmd
shutdown /s /t 0   :: Shutdown immediately
shutdown /r /t 30  :: Restart after 30s
shutdown /a        :: Abort shutdown/restart
```
