# Scenario: "Woluwe" — Too Many Images

**Level:** Medium  
**Time to Solve:** 15 minutes  
**OS:** Debian 13  
**Root (sudo) Access:** Yes

---

## 📘 Description

A pipeline created many Docker images locally for a web application.  
However, **all but one** of these images contain a typo introduced by a developer:

❌ The incorrect images include an instruction that pipes `HelloWorld` into **`index.htmlz`**  
✔️ Only the correct image writes to **`index.html`**

### 🎯 Your Goal

1. **Identify which Docker image does *not* contain the typo** (i.e., the one that uses `index.html`)
2. **Tag the correct image as `prod`**
3. **Deploy it** using: `docker run -d --name prod -p 3000:3000 prod`


<details>
<summary>Solution</summary>
  

Wrote this script below and name it findimage.sh:

```
#!/bin/bash

# Get all image IDs
images=$(docker image ls -q)

for image in $images; do
    echo "Starting container for image: $image"

    # Run container in background
    cid=$(docker run -d $image)
    echo "Container ID: $cid"

    response=$(curl -s http://localhost:3000 || true)

    echo $response

    if [[ "$response" == *"HelloWorld;529"* ]]; then
        echo "Found expected response from image: $image"
        docker tag $image prod:latest

        echo "Stopping script."
        exit 0
    fi

    sleep 1

    # If no match, clean up and continue
    docker rm -f "$cid" >/dev/null 2>&1
    echo "No match for image: $image"
    echo
done

echo "Finished testing all images. No matching output found."

```


`chmod +x findimage.sh`
`./findimage.sh`


</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

