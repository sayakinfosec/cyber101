
## Linux Fundamentals 2

Continuing my Linux journey with Part 2. You will learn how to log into a Linux machine using SSH, work with advanced commands, and explore deeper file system interactions.

---

### 🔑 Accessing Target Linux Machine using SSH

To connect from the **AttackBox** (current machine) to the **Target Linux Machine (10.201.63.XXX):**

```bash
ssh username@targetip
````

**Example session:**

```
root@ip-10-201-87-XX:~# ssh username@10.201.63.XXX
The authenticity of host '10.201.63.XXX (10.201.63.XXX)' can't be established.
ECDSA key fingerprint is SHA256:RQx4gBXmm+WlA6xVjWm6vw66jJGZKGFv8gRkQ9DnlCM8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.201.63.XXX' (ECDSA) to the list of known hosts.
enter password: ...
tryhackme@linux2:~$ 
```

✅ **We are now inside the target machine**

**Q: What is the difference between `~#` and `~$`?**

* `~#` → Root user prompt (superuser).
* `~$` → Normal user prompt.

---
### 😅 Common confusion related to users

#### Linux Prompts & Home

* `root@ip:~#` → logged in as root.
* `bourne@ip:~$` → logged in as normal user.
* `~` → current user’s home. (`/root` for root, `/home/username` for normal users).
* `#` = root prompt, `$` = normal user prompt.


#### Key Directories

* `/` → top of the filesystem (everything).
* `/home` → where normal users live (`/home/bourne`, etc).
* `/root` → root’s private home.

**Access rules:**

* Normal users: full control in their own `/home`.
* Root: can access all, including `/root`.


#### Why below commands will fail

* `cd root/` → wrong path. Needed `/root/`.
* `sudo cd root` → `cd` is a built-in, `sudo` only works with external programs.
* `su cd root` → tried to switch to a user named `cd root` (doesn’t exist).

✅ Correct ways:

* `cd /root` (as root).
* `sudo -i` → become root, then you’re dropped in `/root`.
* `sudo ls /root` → just list contents.


#### Memory Trick

* `/ = everything 🌍`
* `/home = where humans live 🏡`
* `/root = where the boss lives 👑`

---
### ⚡ Introduction to Flags and Switches 

Commands in Linux can be extended with **flags** (short form -a) and **switches** (long form --all). Same Output but notation is different.

Examples:

```bash
ls        # lists visible files in directory
ls -a     # shows all files including hidden ones
ls --help # displays help (options + examples)
```

Note: `--help` is a formatted output of the **man page** (manual).

---

### 📂 File System Interaction (Continuation)

Common file operations:

| Command | Full Name      | Purpose                                   |
| ------- | -------------- | ----------------------------------------- |
| `touch` | touch          | Create a file                             |
| `mkdir` | make directory | Create a folder                           |
| `cp`    | copy           | Copy a file or folder                     |
| `mv`    | move           | Move or rename a file/folder              |
| `rm`    | remove         | Remove a file or folder                   |
| `file`  | file           | Determine the type of a file (e.g., text) |

**⚠️ Important:**

* `rm` removes files directly.
* To delete a **directory**, use `rm -R` (recursive).

**Examples:**

```bash
rm note                # remove file
rm -R mydirectory      # remove directory recursively
```

---

### 📝 Practice Questions

* **How would you create the file named `newnote`?**

  ```bash
  touch newnote
  ```

* **How would we move the file `myfile` to the directory `myfolder`?**

  ```bash
  mv myfile myfolder
  ```

---

### 🔐 Permissions

Permissions are represented as:

```
rwxrwxrwx
```

* **r** → read
* **w** → write
* **x** → execute

**Users & Groups:**

* Files are owned by a **user**.
* A **group** can have different permissions for the same file.

Example: A web server user needs access to files, but customers must only upload their files — permissions prevent cross-access.

---

### 👥 Switching Between Users

Switch user with `su`:

* To switch to `user2`:

  ```bash
  tryhackme@linux2:~$ su user2
  Password:
  user2@linux2:/home/tryhackme$
  For example, when using su to switch to "user2", our new session drops us into our previous user's home directory. 
  ```
* With full login shell (inherits environment):

  ```bash
  tryhackme@linux2:~$ su -l user2
  Password:
  user2@linux2:~$ pwd
  user2@:/home/user2$
  ```

---

### 📁 Common Directories

#### `/etc`

* Stores **system configuration files**.
* Important files:

  * `passwd` → user account info
  * `shadow` → encrypted user passwords (SHA-512)
  * `sudoers` → users allowed to run commands as root

#### `/var`

* "Variable data" → frequently updated files.
* Examples:

  * `/var/log` → system and application logs
  * `/var/tmp` → temporary files

#### `/root`

* Home directory for the **root user**.
* Example contents:

  ```
  myfile myfolder passwords.xlsx
  ```

#### `/tmp`

* Temporary storage (volatile).
* Cleared after system reboot.
* Writable by all users → useful for scripts, but risky.
* What's useful for us in pentesting is that any user can write to this folder by default. Meaning once we have access to a machine, it serves as a good place to store things like our enumeration scripts.
