# "Podgorica": Docker to Podman migration

* **Scenario:** "Podgorica": Docker to Podman migration
* **Level:** Medium
* **Type:** Do
* **Tags:** `advent2025`, `podman`, `systemd`
* **Access:** Email
* **Root (sudo) Access:** True
* **Time to Solve:** 20 minutes

---

### Description

You have been tasked with migrating this future web server from using Docker (which uses a daemon) to **rootless Podman**. 

There is already an Nginx Podman image on the server, and your objective is to manage the container created from it using **systemd**, so that it starts automatically on reboot and continues running unless explicitly stopped (the same behavior expected from a Docker-managed container).

**Your Mission:**
Create a systemd service named `container-nginx.service` that manages the Podman Nginx container. **Enable and start this service.**

> **NOTES:** Although a quadlet file solution should be valid, the check script is still not accounting for it.

There is no need to reboot the VM, although if you want you could reboot it from the command line with `/sbin/shutdown -r now` and refresh or reopen the web console.

### Test

The checker script will test if the `container-nginx.service` is **active and enabled**, and if it can stop and start the service. It will also verify that `curl localhost:8888` returns the default "Welcome to nginx" web page.

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.


<details>
<summary>Solution</summary>
  



</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

