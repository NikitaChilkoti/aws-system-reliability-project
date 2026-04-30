# Incident 4 – Networking Issue

At this point, application was running via systemd, CPU usage was stable and monitoring was active.

Everything looked solid.

And yet… the application suddenly became unreachable again.

---

## Incident Trigger

* Users unable to access application
* Browser showed: **Request timed out**
* No CloudWatch alarms triggered

This was tricky, as I couldn't tick off any obvious failures starting from Step 1

## Step 1: Check EC2 Instance

* Instance State: **Running** 
* Status Checks: **2/2 passed** 

---

## Step 2: Check Application

SSH into instance:

```bash id="n4y2ks"
ssh -i your-key.pem ec2-user@<public-ip>
```

Check service:

```bash id="b2x8fv"
sudo systemctl status flask-app
```

Everything was right- service was running, No errors found 

---

## Step 3: Test locally inside EC2

```
curl http://localhost
```

And I received the response successfully.

> So, the app was working internally… but not externally. That points directly to a network issue.

---

## Step 4: Narrow Down the Problem

At this stage:

* App working 
* EC2 healthy 
* Security group already configured 
* Remaining suspect: **Network ACL (NACL)**

---

## Step 5: Investigate NACL

* Navigated to VPC → Network ACLs
* Checked rules associated with subnet

### And I found the problem:

Inbound Rule:

* Deny HTTP (port 80)

Outbound Rule:

* Restricted ephemeral ports

> NACL was blocking traffic.

---

## Step 6: Fix NACL Rules

Updated rules:

### Inbound:

| Rule | Type | Port | Allow/Deny |
| ---- | ---- | ---- | ---------- |
| 100  | HTTP | 80   | ALLOW      |

### Outbound:

| Rule | Type | Port Range | Allow/Deny |
| ---- | ---- | ---------- | ---------- |
| 100  | ALL  | 0-65535    | ALLOW      |

---

## Step 7: Validate Fix

* Accessed application via browser
* Application loaded successfully

---

## Finally, after resolving multiple incidents, the system was finally stable.

But stability is not the end goal.
