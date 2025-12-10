# "Socorro, NM": Optimize Podman image

* **Scenario:** "Socorro, NM": Optimize Podman image
* **Level:** Medium
* **Type:** Do
* **Tags:** `podman`, `advent2025`
* **Access:** Email
* **Root (sudo) Access:** True
* **Time to Solve:** 10 minutes

---

### Description

The Podman image `localhost/prod:latest` contains a static website. Initially, the image size is **261 MB** and contains **100 layers**. 

**Your Task:**

1.  Optimize the image `localhost/prod:latest` so that its size is **less than 200 MB**, using the same tag.
2.  Run a container named "check" from the optimized image:
    `podman run -d --name check -p 8888:80 localhost/prod:latest`
    so that `curl localhost:8888` returns **100 lines**.

### Test

The Podman image `localhost/prod:latest` size must be **less than 200 MB**, and running `curl localhost:8888` from a container named "check" created from the image must return **100 lines**.

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.



<details>
<summary>Solution</summary>
  



</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

