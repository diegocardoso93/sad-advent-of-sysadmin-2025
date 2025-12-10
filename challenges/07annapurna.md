# "Annapurna": High privileges

* **Scenario:** "Annapurna": High privileges
* **Level:** Medium
* **Type:** Hack
* **Tags:** `hack`, `advent2025`
* **Access:** Email
* **Root (sudo) Access:** False
* **Time to Solve:** 20 minutes

---

### Description

You are logged in as the user `admin`.



You have been tasked with auditing the admin user privileges in this server; "admin" should not have sudo (root) access.

**Your Mission:** Exploit this server so you as the admin user can read the file `/root/mysecret.txt`.

Save the content of `/root/mysecret.txt` to the file `/home/admin/mysolution.txt`, for example:
`echo "secret" > ~/mysolution.txt`

### Test

Running the following command:
`md5sum /home/admin/mysolution.txt`

Should return:
`47ee165a2262476f6866902a93f2a41d`
*(We also accept the md5sum of the same file without a newline at the end).*

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.


<details>
<summary>Solution</summary>
  



</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

