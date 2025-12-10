# "Torino": Optimize grande Docker image

* **Scenario:** "Torino": Optimize grande Docker image
* **Level:** Medium
* **Type:** Do
* **Tags:** `advent2025`, `docker`, `node.js`
* **Access:** Email
* **Root (sudo) Access:** True
* **Time to Solve:** 20 minutes

---

### Description

A Torino Node.js application is located in the `~torino-app` directory.

You can run it directly with:
`nohup node app.js > app.log 2>&1 &`

You can also verify that it works by running:
`curl localhost:3000`

There is already a `torino` Docker image built with the Dockerfile in `~torino-app`, but the resulting image size is **916 MB**.



**Your Task is to optimize the Docker image size:**

1.  Build a new Docker image for the Torino application, also called `torino:latest`, but with a total size **under 122 MB**.
2.  Create and run a container using this optimized image.

> **NOTE:** You can only use the existing Docker images in the server. To build a Node application, you need to `COPY` the `app.js` and `package*.json` files. Since you cannot `RUN npm install` without Internet access, you must also copy the pre-existing `node_modules` directory.

### Test

The `torino` Docker image must be **less than 122 MB**, and the running container must successfully respond to:
`curl http://localhost:3000`

Which should return:
`Hello from Torino!`

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.



<details>
<summary>Solution</summary>
  



</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

