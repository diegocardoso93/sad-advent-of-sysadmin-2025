# 🧩 Challenge: Kortenberg: Can't touch this!

| Property | Value |
| :--- | :--- |
| **Day** | 2025-12-03 |
| **Level** | Easy |
| **Type** | Fix |
| **Time to Solve** | 15 minutes |
| **Access** | Email |

## 📝 Description

Is "All I want for Christmas is you" already everywhere? A bit unrelated, someone messed up the permissions in this server, the *admin* user can't list new directories and can't write into new files. Fix the issue.

> **NOTE:** Besides solving the problem in your current admin shell session, you need to fix it permanently, as in a new login shell for user admin (like the one initiated by the scenario checker) should have the problem fixed as well.

## ✅ Test

The *admin* user in a separate Bash login session should be able to create a new directory in your `/home/admin` directory, as well as being able to create a file into this new directory and add text into the new file.

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.



<details>
<summary>Solution</summary>


The problem stemmed from an incorrect Apache configuration pointing to the wrong PHP-FPM port, followed by SELinux blocking Apache even after the port was fixed.

### 1️⃣ **Locate Incorrect Handler Configuration**

Search for `SetHandler` directives to find where the FastCGI port is defined:

```bash
grep -R "SetHandler" /etc/httpd /etc/httpd/conf* -n
```

The configuration was pointing PHP requests to port **9001**, while PHP-FPM was actually running on **9000**.

---

### 2️⃣ **Update the Port Configuration**

Correct the configuration to point to the right PHP-FPM port (`127.0.0.1:9000`), then test:

```bash
curl localhost/index.php | head
```

---

### 3️⃣ **Check Apache Logs for New Errors**

After fixing the port, Apache reported:

```
(13)Permission denied: FCGI: attempt to connect to 127.0.0.1:9000 failed
SELinux policy enabled; httpd running as context httpd_t
```

This indicates **SELinux** is blocking Apache from connecting to PHP-FPM.

---

### 4️⃣ **Fix SELinux Policy**

Allow Apache to establish network connections:

```bash
sudo setsebool -P httpd_can_network_connect on
```

The `-P` option makes this change persistent across restarts and new login shells, satisfying the challenge requirement.

---

### 5️⃣ **Restart Apache**

```bash
sudo systemctl restart httpd
```

---

## ✅ **Final Result**

With the port corrected and SELinux updated, the `admin` user can now:

* Create new directories under `/home/admin/`,
* Create and write to files in those directories,
* And PHP-FPM works without permission issues.

</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)
