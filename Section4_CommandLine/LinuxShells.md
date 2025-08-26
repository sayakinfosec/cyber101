## Linux Shells

### 💎 Introduction to Linux Shells

* GUI = easy but limited control (like ordering food from a menu).
* CLI (via shells) = powerful, efficient, resource-friendly (like cooking yourself, with shell giving recipes).
* Shells = programs that let users interact with the OS kernel through commands.
* Commonly used by developers, sysadmins, and hackers (why we see them in movies).

---

### 💎 Types of Linux Shells

**Check Current Shell:**

```bash
echo $SHELL
```

**List Installed Shells:**

```bash
cat /etc/shells
```

**Switch Shell Temporarily:**

```bash
zsh
```

**Change Default Shell Permanently:**

```bash
chsh -s /usr/bin/zsh
```

#### **Bash (Bourne Again Shell)**

* Default on most distros.
* Combines features of older shells (`sh`, `ksh`, `csh`).
* Features: scripting, tab completion, command history (`history`).

#### **Fish (Friendly Interactive Shell)**

* Not default, but very user-friendly.
* Features: simple syntax, auto spell correction, themes, syntax highlighting.
* Also supports scripting, history, and completion.

#### **Zsh (Z Shell)**

* Advanced, modern shell (not default).
* Combines Bash features + extras.
* Features: advanced completion, auto spell correction, high customization (via *oh-my-zsh*).

**Comparison Table:**

| Feature             | Bash 🐚                    | Fish 🐟                      | Zsh ✨                         |
| ------------------- | -------------------------- | ---------------------------- | ----------------------------- |
| Full Name           | Bourne Again Shell         | Friendly Interactive Shell   | Z Shell                       |
| Scripting           | Widely compatible          | Limited                      | Excellent (Bash + extras)     |
| Tab Completion      | Basic                      | Advanced (smart suggestions) | Advanced (plugins)            |
| Customization       | Basic                      | Moderate (themes)            | Extensive (oh-my-zsh)         |
| User Friendliness   | Traditional, less friendly | Most user-friendly           | Very friendly w/customization |
| Syntax Highlighting | ❌                          | ✅ Built-in                   | ✅ via plugins                 |

---

### 💎 Shell Scripting Basics

**Create Script:**

```bash
nano first_script.sh
```

**Shebang (defines interpreter):**

```bash
#!/bin/bash
```

**Make Script Executable:**

```bash
chmod +x first_script.sh
./first_script.sh
```

---

### 💎 Variables & Input

```bash
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```

---

### 💎 Loops

```bash
#!/bin/bash
for i in {1..10}; do
  echo $i
done
```

**Math in Bash:**

```bash
let "x = (2+3)*4"   # 20
x=$((2+3)*4)        # 20
```

---

### 💎 Conditionals

```bash
#!/bin/bash
echo "Enter your name:"
read name

if [ "$name" = "Stewart" ]; then
    echo "Welcome Stewart! Here is the secret: THM_Script"
else
    echo "Sorry! You are not authorized."
fi
```

---

### 💎 Comments

```bash
# This is a comment
# Use them to explain important parts of scripts
```

---

### 💎 The Locker Script (Combining Variables, Loops, Conditionals)

```bash
#!/bin/bash 

# Defining variables
username=""
companyname=""
pin=""

# Loop for input
for i in {1..3}; do
    if [ "$i" -eq 1 ]; then
        echo "Enter your Username:"
        read username
    elif [ "$i" -eq 2 ]; then
        echo "Enter your Company name:"
        read companyname
    else
        echo "Enter your PIN:"
        read pin
    fi
done

# Authentication check
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```

---

✅ **Review Questions Recap:**

* Which shell has syntax highlighting built-in? → **Fish**
* Which shell has no auto spell correction? → **Bash**
* Which command shows all previous commands? → **history**
