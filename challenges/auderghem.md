# 🧩 Challenge: Auderghem: containers miscommunication

| Property | Value |
| :--- | :--- |
| **Day** | 2025-12-01 |
| **Level** | Easy |
| **Type** | Fix |
| **Time to Solve** | 30 minutes |
| **Access** | Email |

## 📝 Description

There is an Nginx Docker container that listens on port 80, the purpose of which is to redirect the traffic to two other containers (*statichtml1* and *statichtml2*), but it's not working. Fix the problem.

> **IMPORTANT:** You can restart all containers, but do not **stop** or **remove** them.

## ✅ Test

The Nginx container must redirect the traffic to the *statichtml1* and *statichtml2* containers.

* `curl http://localhost` returns `Welcome to nginx`
* `curl http://localhost/1` returns `HelloWorld;1`
* `curl http://localhost/2` returns `HelloWorld;2`

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.

---

[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)
