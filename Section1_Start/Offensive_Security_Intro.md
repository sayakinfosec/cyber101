
## Offensive Security Intro  

### 🎯 Objective  
Hack a website in a safe lab environment to simulate an ethical hacker’s job.  

### 🛠 Tools Used  
- Gobuster  

### 💻 Command Executed  
```
gobuster -u http://fakebank.thm -w wordlist.txt dir
```

### 🔍 Command Breakdown

-u → Specifies the target URL (http://fakebank.thm)

-w → Wordlist file to brute-force directories (wordlist.txt)

dir → Specifies the mode. in this case, directory brute-forcing mode.

**NOTE**: 
Gobuster has multiple modes, for example:

dir → brute-force directories and files on websites

dns → brute-force subdomains of a domain

vhost → brute-force virtual hosts

s3 → brute-force Amazon S3 buckets

fuzz → fuzz arbitrary locations


### 📂 Output Findings
```
/images (Status: 301)
/bank-transfer (Status: 200)
```

### 📝 Observations

The /images directory exists but redirects (301).

The /bank-transfer endpoint is accessible (200 OK).

/bank-transfer is likely the vulnerable endpoint that could be exploited further.
