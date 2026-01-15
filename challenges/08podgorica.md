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
  

1) Confirm which Nginx image already exists and create the container **(without starting it)** with the name **nginx** and port **8888:80**:
```bash
podman images | grep -i nginx

# use o NOME:TAG que aparecer aí (exemplo abaixo)
podman rm -f nginx 2>/dev/null || true
podman create --name nginx -p 8888:80 docker.io/library/nginx:latest
```

2) Generate the systemd unit with the expected name **container-nginx.service** (default name in `podman generate systemd`), install it as a **user service**, and reload it.
```bash
mkdir -p ~/.config/systemd/user
cd ~/.config/systemd/user
podman generate systemd --name nginx --files --restart-policy=always

systemctl --user daemon-reload
```

3) Enable "linger" to allow the user to start services **on boot even without logging in**, and then **enable and start** the service:
```bash
sudo loginctl enable-linger "$USER"
systemctl --user enable --now container-nginx.service
```

4) Check (including the curl test):
```bash
systemctl --user is-enabled container-nginx.service
systemctl --user is-active container-nginx.service
curl -s localhost:8888 | grep -i "Welcome to nginx"
```


</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

