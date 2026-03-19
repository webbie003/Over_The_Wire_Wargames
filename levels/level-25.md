# Bandit Level 25 → Level 26

## Level Goal / Objective

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not `/bin/bash`, but something else. Find out what it is, how it works and how to break out of it.

🔗 https://overthewire.org/wargames/bandit/bandit26.html

## Commands You May Need

```text
ssh , cat , more , vi , ls , id , pwd
```

## Concept Focus

* Inspecting user shells
* Understanding wrapper programs
* Enumerating accessible files
* Preparing alternate credentials for the next level

## Approach

### 1. Connect to the Level

Log in via SSH using the credentials from the previous level.

---

### 2. Identify the Target Shell

Inspect `/etc/passwd` to see what shell is configured for `bandit26`.

```bash
cat /etc/passwd
```

This shows that `bandit26` does not use a normal interactive shell.

---

### 3. Inspect the Wrapper Program

View the wrapper binary/script used as the shell:

```bash
cat /usr/bin/showtext
```

This reveals that it launches `more` against a text file instead of starting a normal shell.

---

### 4. Enumerate Accessible Files

From there, inspect nearby directories and files to see what is available:

```bash
ls /home/
ls -la /home/bandit25
```

This exposes a file named `bandit26.sshkey` that can be read from the current account.

---

### 5. Retrieve the Next Credential

Read the SSH private key that will be used to access the next level:

```bash
cat /home/bandit25/bandit26.sshkey
```

---

## Walkthrough (Screenshots)

![Screen Capture](../images/level-25-01.png)

---

## Credential for Level 26

```text
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

---

## Key Takeaways

* Always check a user’s configured shell in `/etc/passwd`
* Wrapper programs often reveal how access is being restricted
* Non-standard shells can still expose useful files and paths
* Enumeration is often enough to recover the next credential
