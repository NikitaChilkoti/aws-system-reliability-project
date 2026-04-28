# Incident 1 – Application Not Accessible

## With monitoring in place, the system was now under observation.

And as expected…
it didn’t take long for the first alert to arrive.

---

## Incident Trigger
An email alert was received from CloudWatch:

> **Alarm:** StatusCheckFailed
> **State:** ALARM

This indicated that something was wrong with the instance or its accessibility.

---

## Step 1: Initial Checks

### Check EC2 Instance Status

* Instance State: **Running**
* Status Checks: **2/2 passed**

> Instance looked healthy… so the problem was hiding somewhere else.

---

### Check Application Status

SSH into the instance:

```bash id="k2n8sd"
ssh -i your-key.pem ec2-user@<public-ip>
```

Verify if the Flask app is running:

```bash id="m4xq1p"
ps aux | grep python
```

Result:

* Application was running

> So the app is live…

---

## Step 2: Network-Level Investigation

At this point, the issue likely involved **network access**.

### Check Security Group

* Navigated to EC2 → Security Groups
* Reviewed inbound rules

### And I found the problem:

* Port 80 (HTTP) was **not allowed**

> The app was running perfectly… just locked inside like a VIP with no guest list.
