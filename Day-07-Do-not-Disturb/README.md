# Day 07 - Do Not Disturb

## 🔗 Room Link
https://tryhackme.com/room/donotdisturb

---

# 📝 Room Overview

**Difficulty:** Medium  
**Category:** Web Exploitation | NoSQL Injection | SSTI | Privilege Escalation | Linux

In this room, I explored how multiple small vulnerabilities can be chained together to fully compromise a Linux machine.

The attack started with a **NoSQL authentication bypass**, followed by **Server-Side Template Injection (SSTI)** in an EJS application to gain remote code execution. After obtaining the first shell, I performed privilege escalation by pivoting into another service account and finally recovered the **root flag** through raw disk access.

---

## 📸 Room Overview

> *(Add Room Overview Screenshot Here)*

```
images/room-overview.png
```

---

# 🎯 Objective

The objective of this room was to:

- Discover a NoSQL authentication bypass.
- Login as a privileged staff user.
- Exploit an EJS template injection vulnerability.
- Gain an initial reverse shell.
- Escalate privileges to a higher privileged service account.
- Recover both **user.txt** and **root.txt**.

---

# 🔍 Enumeration

I first configured **Burp Suite** with **FoxyProxy** to intercept the login request.

While testing different usernames, I noticed that the account named:

```
attendant
```

behaved differently from the other users.

Instead of validating the password correctly, the backend accepted MongoDB operators.

---

## 📸 Burp Login Request

> *(Add Burp Login Screenshot Here)*

```
images/burp-login.png
```

---

# 🚀 NoSQL Authentication Bypass

The application was vulnerable to a NoSQL Injection.

Instead of sending a normal password value, I replaced it with a MongoDB operator.

Example:

```
username=attendant
password[$ne]=test
```

The `$ne` operator means **Not Equal**, allowing the password validation to evaluate as true.

After forwarding the modified request through Burp Suite, I successfully logged in as the **staff** user.

---

# 📸 Staff Login

> *(Add Staff Dashboard Screenshot Here)*

```
images/staff-login.png
```

---

# 💻 Remote Code Execution (EJS SSTI)

Once logged in, I discovered that the application was using **EJS templates**.

By injecting server-side JavaScript, I confirmed that the application was vulnerable to **Server-Side Template Injection (SSTI)**.

The vulnerable functionality allowed arbitrary NodeJS code execution.

I then prepared a reverse shell payload and started a Netcat listener.

```
nc -lvnp 4545
```

After triggering the payload, I received my first reverse shell.

---

## 📸 First Reverse Shell

> *(Add Reverse Shell Screenshot Here)*

```
images/reverse-shell.png
```

---

# 👤 User Flag

With shell access, I searched for the user flag.

A wildcard search quickly located the file.

```
cat /*/*/user.txt
```

The first flag was successfully recovered.

---

## 📸 User Flag

> *(Add User Flag Screenshot Here)*

```
images/user-flag.png
```

---

# ⬆ Privilege Escalation

The initial shell was running with limited privileges.

After further enumeration, I identified another execution path that allowed me to obtain a second reverse shell.

This time, the shell was spawned as:

```
pipeline-service
```

instead of the original low-privileged account.

A second Netcat listener was started.

```
nc -lvnp 4546
```

The NodeJS exploit script was then executed to establish the new connection.

```
node /tmp/ins.js
```

---

## 📸 Pipeline Service Shell

> *(Add Privilege Escalation Screenshot Here)*

```
images/pipeline-shell.png
```

---

# 💾 Raw Disk Enumeration

After gaining access as **pipeline-service**, I enumerated the available storage devices.

```
lsblk
```

The system exposed the root filesystem through a block device.

Example:

```
/dev/nvme0n1p1
```

Since the service account had sufficient access, it was possible to inspect the filesystem directly and recover the root flag.

---

## 📸 Disk Enumeration

> *(Add lsblk Screenshot Here)*

```
images/lsblk.png
```

---

# 👑 Root Flag

Using the available raw disk access, I successfully recovered the root flag from the filesystem.

---

## 📸 Root Flag

> *(Add Root Flag Screenshot Here)*

```
images/root-flag.png
```

---

# 🛠 Tools Used

- Burp Suite
- FoxyProxy
- Netcat
- NodeJS
- Linux Terminal

---

# 📚 Key Concepts Learned

✔ NoSQL Injection

✔ MongoDB Operators (`$ne`)

✔ Burp Suite Request Manipulation

✔ EJS Server-Side Template Injection (SSTI)

✔ Reverse Shells

✔ Linux Enumeration

✔ Privilege Escalation

✔ Raw Disk Enumeration

✔ Root Flag Recovery

---

# 💡 What I Learned

This room demonstrated how multiple low-to-medium severity vulnerabilities can be chained together to completely compromise a machine.

The biggest takeaway for me was that exploiting a system is rarely about finding a single vulnerability—it’s about combining multiple weaknesses, including authentication bypasses, insecure template rendering, and excessive local privileges, to achieve full system compromise.

---

# 🔗 Connect With Me

### 💼 LinkedIn

*(Add LinkedIn Walkthrough Post Here)*

**LinkedIn Post:**  
<PASTE_YOUR_LINKEDIN_POST_LINK_HERE>

### 💻 GitHub

https://github.com/techyprachi

---

⭐ If you found this write-up helpful, feel free to explore the rest of my **TryHackMe Hacker Holidays 2026** series on GitHub!
