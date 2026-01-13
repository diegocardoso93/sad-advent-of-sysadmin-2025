# "Sumé": Tied in a Knot

* **Scenario:** "Sumé": Tied in a Knot
* **Level:** Medium
* **Type:** Fix
* **Tags:** `advent2025`, `dns`
* **Access:** Email
* **Root (sudo) Access:** True
* **Time to Solve:** 20 minutes

---

### Description

A DNS server running Knot DNS is serving the zone sadservers.internal (see ls /var/lib/knot/zones/), but users are reporting that they cannot access blog.sadservers.internal neither api.sadservers.internal. Your task is to diagnose and fix the DNS issues so the services become accessible.
You can manage Knot DNS with sudo knotc commands.

Note: the 203.0.113.0/24 range is part of TEST-NET-3, a block reserved by RFC 5737 for documentation and examples, making it a Bogon IP range.

IMPORTANT. Do not change the Nginx configurations under /opt/services/ for the solution to work.

### Test

You are able to access the blog and the API services: curl blog.sadservers.internal returns Welcome to blog.sadservers.internal
curl api.sadservers.internal returns {"status": "ok", "service": "api.sadservers.internal"}

The "Check My Solution" button runs the script /home/admin/agent/check.sh, which you can see and execute.



<details>
<summary>Solution</summary>
  



</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

