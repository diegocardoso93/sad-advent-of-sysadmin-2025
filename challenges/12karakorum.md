# "Karakorum": WTFIT – What The Fun Is This?

* **Scenario:** "Karakorum": WTFIT – What The Fun Is This?
* **Level:** Hard
* **Type:** Do
* **Tags:** `realistic-interviews`, `pro`, `linux-other`, `advent2025`
* **Access:** Email
* **Root (sudo) Access:** True
* **Time to Solve:** 20 minutes

---

### Description

(NOTE: this is not a new scenario but an existing Pro one temporarily available to all users as the last Advent of SysAdmin 2025 scenario).

There's a binary at /home/admin/wtfit that nobody knows how it works or what it does ("what the fun is this"). Someone remembers something about wtfit needing to communicate to a service in order to start.

Run this wtfit program so it doesn't exit with an error, fixing or working around things that you need but are broken in this server.

### Test

Running /home/admin/wtfit returns OK.


<details>
<summary>Solution</summary>
  


sudo python3 - << 'EOF'
import os
os.chmod("/home/admin/wtfit", 0o755)
EOF

strings /home/admin/wtfit | grep -i config


touch /home/admin/wtfitconfig.conf


admin@ip-10-1-12-42:~$ /home/admin/wtfit
ERROR: can't connect to server


</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

