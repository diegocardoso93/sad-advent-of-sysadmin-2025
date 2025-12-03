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

---

[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)