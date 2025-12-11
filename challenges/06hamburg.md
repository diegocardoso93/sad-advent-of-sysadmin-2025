# "Hamburg": Find the AWS EC2 volume

* **Scenario:** "Hamburg": Find the AWS EC2 volume
* **Level:** Easy
* **Type:** Do
* **Tags:** `json`, `advent2025`
* **Access:** Email
* **Root (sudo) Access:** True
* **Time to Solve:** 30 minutes

---

### Description

We have a lot of AWS EC2 instances and EBS volumes. The description of which volumes we have is saved to a file using the command:
`aws ec2 describe-volumes > aws-volumes.json`



One of the volumes attached to an ec2 instance contains important data and we need to identify which instance it is attached to (its **InstanceId**). We only remember the following characteristics of the target volume:

* **Type:** gp3
* **Created before:** 31/09/2025
* **Size:** < 64
* **Iops:** < 1500
* **Throughput:** > 300

**Your Mission:**
Find the correct instance and put its "InstanceId" into the `~/mysolution` file.
*Example:* `echo "i-00000000000000000" > ~/mysolution`

### Test

As the user "admin", run the following command:
`md5sum /home/admin/mysolution`

It should return:
`e7e34463823bf7e39358bf6bb24336d8`
*(we also accept the file without a new line at the end).*

The "Check My Solution" button runs the script `/home/admin/agent/check.sh`, which you can see and execute.



<details>
<summary>Solution</summary>
   
   
`jq` is ideal for querying structured JSON. The following one-liner filters all volumes and extracts the `InstanceId` from the correct match.

### **Run this command:**

```bash
jq -r '
  .Volumes[]
  | select(.VolumeType=="gp3")
  | select(.CreateTime < "2025-09-31")
  | select(.Size < 64)
  | select(.Iops < 1500)
  | select(.Throughput > 300)
  | select(.Attachments | length > 0)
  | .Attachments[0].InstanceId
' aws-volumes.json > ~/mysolution
```

---

# **📌 Explanation of Filters**

| Condition                 | jq Expression                        |              |
| ------------------------- | ------------------------------------ | ------------ |
| Type = gp3                | `select(.VolumeType=="gp3")`         |              |
| Created before 31-09-2025 | `select(.CreateTime < "2025-09-31")` |              |
| Size < 64                 | `select(.Size < 64)`                 |              |
| IOPS < 1500               | `select(.Iops < 1500)`               |              |
| Throughput > 300          | `select(.Throughput > 300)`          |              |
| Must be attached          | `select(.Attachments                 | length > 0)` |
| Extract instance          | `.Attachments[0].InstanceId`         |              |

---

# **✅ Validation**

To confirm your solution, run:

```bash
md5sum ~/mysolution
```

Expected output:

```
e7e34463823bf7e39358bf6bb24336d8
```


</details>

---


[⬅️ Back to Advent of Sysadmin 2025 README](../README.md)

