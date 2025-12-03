# 🧩 Challenge: Marseille: Rocky Security

| Property | Value |
| :--- | :--- |
| **Day** | 2025-12-02 |
| **Level** | Medium |
| **Type** | Fix |
| **Time to Solve** | 15 minutes |
| **Access** | Email |

## 📝 Description

As the Christmas shopping season approaches, the security team has asked Mary and John to implement more security measures. Unfortunately, this time they have broken the LAMP stack; the frontend is unable to get an answer from upstream, thus they need your help again to fix it.

The application should be able to serve the content from the webserver.

> **Note for Pro users:** Direct SSH access is not available (yet) for this scenario.

## ✅ Test

* `curl localhost | head -n1` returns `SadServers - LAMP Stack`

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