# 🧩 Challenge: Auderghem: containers miscommunication

[https://sadservers.com/scenario/auderghem](https://sadservers.com/scenario/auderghem)

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

<details>
<summary>Solution</summary>


### 1\. Problem Identification and Diagnosis

The core problem is that the Nginx container, configured as a reverse proxy, is unable to correctly route traffic to the `statichtml1` and `statichtml2` containers.

Upon inspecting the existing Nginx configuration file (`~/app/default.conf`):

```nginx
server {
    listen 80;
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }
    location /1 {
        rewrite ^ / break;
        proxy_pass http://statichtml1.sadservers.local;
        proxy_connect_timeout 2s;
        proxy_send_timeout 2s;
        proxy_read_timeout 2s;
    }
    location /2 {
        rewrite ^ / break;
        proxy_pass http://statichtml2.sadservers.local;
        proxy_connect_timeout 2s;
        proxy_send_timeout 2s;
        proxy_read_timeout 2s;
    }
}
```

The issue has two parts:

1.  **Incorrect Hostname/Port:** The `proxy_pass` directives use fully qualified domain names (FQDNs) like `statichtml1.sadservers.local` and assume port 80, which are incorrect for internal Docker networking in this setup. The target containers are actually running on **port 3000** and are internally resolvable by their container names.
2.  **Network Isolation:** The Nginx container is not connected to the Docker network (`app-static`) shared by the two target containers (`statichtml1` and `statichtml2`).

Checking the containers confirms the correct names and ports:

```bash
admin@i-03fc55a1924a48445:~/app$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS         PORTS                         NAMES
89bf0e394bb9   statichtml:2          "busybox httpd -f -v…"   26 hours ago   Up 10 minutes   3000/tcp                      statichtml2
1f96c1876662   statichtml:1          "busybox httpd -f -v…"   26 hours ago   Up 10 minutes   3000/tcp                      statichtml1
7440094fc321   nginx                 "/docker-entrypoint.…"   26 hours ago   Up 10 minutes   0.0.0.0:80->80/tcp, [::]:80->80/tcp   nginx
```

-----

### 2\. Solution Steps

#### A. Connect Nginx to the Correct Network

First, connect the `nginx` container to the network where the target containers reside (`app-static`).

1.  **Identify the network** (using `docker network ls` or inspection).
2.  **Connect Nginx:**
    ```bash
    docker network connect app-static nginx
    ```

#### B. Update Nginx Configuration

Next, modify the `~/app/default.conf` file to use the correct internal container names and port (3000).

The full updated configuration in `~/app/default.conf`:

```nginx
server {
    listen 80;
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }
    location /1 {
        rewrite ^ / break;
        # Changed FQDN and default port to internal container name and correct port (3000)
        proxy_pass http://statichtml1:3000/; 
        proxy_connect_timeout 2s;
        proxy_send_timeout 2s;
        proxy_read_timeout 2s;
    }
    location /2 {
        rewrite ^ / break;
        # Changed FQDN and default port to internal container name and correct port (3000)
        proxy_pass http://statichtml2:3000/;
        proxy_connect_timeout 2s;
        proxy_send_timeout 2s;
        proxy_read_timeout 2s;
    }
}
```

#### C. Apply Configuration Changes

Reload the Nginx configuration inside the running container to apply the changes without a full restart:

```bash
docker exec nginx nginx -s reload
```

-----

### 3\. Verification

The Nginx container can now correctly resolve the target containers and proxy the requests, meeting the test criteria.

| Request | Expected Output |
| :--- | :--- |
| `curl http://localhost` | `Welcome to nginx` |
| `curl http://localhost/1` | `HelloWorld;1` |
| `curl http://localhost/2` | `HelloWorld;2` |

</details>

---

[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

