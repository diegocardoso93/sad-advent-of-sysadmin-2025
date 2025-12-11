# "La Rinconada": Elevating privileges

* **Scenario:** "La Rinconada": Elevating privileges
* **Level:** Medium
* **Type:** Hack
* **Tags:** `hack`, `advent2025`
* **Access:** Email
* **Root (sudo) Access:** False
* **Time to Solve:** 15 minutes

---

### Description

You are logged in as the user "admin" without general "sudo" privileges.
The system administrator has granted you limited "sudo" access; this was intended to allow you to read log files.

**Your mission:** Find a way to exploit this limited sudo permission to gain a full root shell and read the secret file at `/root/secret.txt`.

Copy the content of `/root/secret.txt` into the `/home/admin/solution.txt` file, for example:
`cat /root/secret.txt > /home/admin/solution.txt`
(the "admin" user must be able to read the file).

### Test

As the user "admin", the command:
`md5sum /home/admin/solution.txt`

Should return:
`52a55258e4d530489ffe0cc4cf02030c`
*(we also accept the hash of the same secret string without an ending newline).*

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.


<details>
<summary>Solution</summary>
  

### Scenario Overview

You are logged in as the user **`admin`** on a Debian 13 system. You do not have general `sudo` privileges. The system administrator, however, has granted you limited `sudo` access, intended only for reading log files.

Your mission is to exploit this limited `sudo` permission to gain a full root shell and read the secret file at `/root/secret.txt`, placing its content into `/home/admin/solution.txt`.

### Step-by-Step Solution

The core of the exploit lies in using the limited `sudo` permission to invoke an application that allows for command execution or arbitrary file manipulation, in this case, a pager/editor used for viewing log files.

#### 1\. Identify the Limited Sudo Permission

First, check what commands the `admin` user is allowed to run via `sudo`.

```bash
sudo -l
```

**Expected Output (or similar):**

```
User admin may run the following commands on hostname:
    (root) /usr/bin/less /var/log/*
    (root) /usr/bin/cat /var/log/*
    (root) /usr/bin/more /var/log/*
```

The crucial permission is allowing `admin` to run a pager like `/usr/bin/less` (or `/usr/bin/more`) or a viewer like `/usr/bin/cat` as `root` for files within `/var/log/`.

#### 2\. Exploit the `less` Pager

The `less` pager (and often `more` as well) allows the user to open a file and, crucially, supports invoking an external editor or shell commands while viewing the file.

We will use `sudo` to open any existing log file in `/var/log/` using `less` as the `root` user.

**Command to start the exploitation:**

```bash
sudo /usr/bin/less /var/log/alternatives.log
```

(You can use any existing file inside `/var/log/`, e.g., `dmesg`, `syslog`, etc.)

#### 3\. Execute Commands within `less`

Once the file is open in `less` (running as `root`), you can use its interactive commands to execute arbitrary commands.

  * Type **`v`** (to invoke the default editor, usually `nano`).
  * The `nano` editor will open, running with **`root`** privileges.

#### 4\. Execute Arbitrary Commands using `nano`

Inside the `nano` editor (which is running as `root`):

1.  Press **`Ctrl` + `R`** (Read File).

2.  Press **`Ctrl` + `X`** (Execute Command).

3.  The editor will prompt for a command to execute. Enter the following command to complete the mission:

    ```bash
    cp /root/secret.txt /home/admin/solution.txt && chmod 644 /home/admin/solution.txt
    ```

    *This command copies the secret file and ensures the `admin` user has read permission on the solution file.*

4.  Press **`Enter`** to execute the command.

5.  Wait for the command to execute (it will likely return you to the `nano` editor).

6.  Press **`Ctrl` + `X`** to exit `nano`.

7.  Press **`q`** to quit `less`.

#### 5\. Verification

You are now back at the `admin` shell. The `solution.txt` file should contain the secret content.

```bash
# Verify the content
cat /home/admin/solution.txt

# Run the provided check script
/home/admin/agent/check.sh

# Or verify the MD5 hash
md5sum /home/admin/solution.txt
# Expected hash: 52a55258e4d530489ffe0cc4cf02030c
```


</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

