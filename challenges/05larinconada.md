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
  



</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

