# Day 1 – The Concierge Knows Too Much

## Room Information

- Platform: TryHackMe
- Event: Hacker Holidays 2026
- Difficulty: Easy
- Category: Information Disclosure

---

## Room Overview

This room focuses on identifying exposed information through basic web enumeration.

---

# Methodology

## Step 1 – Explore the Target

I started by visiting the web application and reviewing the available pages.

### Screenshot

![Room Overview](images/room-overview.png)

---

## Step 2 – Read the Room Description

The room description provided useful context about the challenge objectives.

### Screenshot

![Room Description](images/room-description.png)

---

## Step 3 – Initial Enumeration

I inspected the Task and looked for publicly exposed information.

### Screenshot

![Task](images/Task.png)

---

## Step 4 – Inspect the Important Information

Developer comments contained information that could assist in gaining access.

### Screenshot

![Important Info](images/Important-Info.png)

---

![Agent](images/Agent.png)

---

## Security Finding

- Information Disclosure
- Exposed Credentials
- Weak Operational Security
-
![Message](images/message.png)

---

## MITRE ATT&CK

- T1592 - Gather Victim Host Information
- T1580 - Gather Victim Network Information

-
![Flag](images/Flag.png)
---

## Lessons Learned

- Always inspect HTML source.
- Small information leaks can become entry points.
- Enumeration should precede exploitation.

---

> **Disclaimer**
>
> This write-up is intended for educational purposes. Flags, credentials, and challenge answers have been intentionally omitted in accordance with TryHackMe's write-up guidelines.
