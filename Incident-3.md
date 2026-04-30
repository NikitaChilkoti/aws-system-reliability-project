## Incident 3 – Application Crash

The infrastructure was stable.
Monitoring was active.
CPU issues were resolved.

I was calm, or I would better say suspiciously calm. :D

Until the next alert arrived.

---

## Incident Trigger

Users were unable to access the application.

* Browser showed: **Connection refused / timeout**
* No CloudWatch CPU or status alarms triggered

> I immediately checked infra, and it was fine… but the app wasn’t responding. Keep reading to know what happened.

## Step 1: Verify EC2 Instance

* Instance State: **Running** 
* Status Checks: **2/2 passed** 

> Infrastructure looked perfectly healthy.

---

## Step 2: Check Application Process

SSH into the instance:

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

Check running processes:

```bash
ps aux | grep python
```

### So, the peoblem was here. Flask application process was **NOT running**

> The server was alive… the app had crashed.

---

## Step 3: Check Application Logs (Basic)

Since the app was started manually earlier, there were no structured logs.

> Classic situation:
> “Something crashed… but left no explanation behind.”

---

## Step 4: Temporary Fix (Manual Restart)

Restarted the application:

```bash
sudo python3 app.py
```

* Application became accessible again

---

## Problem Identified

> Application was running manually, as no auto-restart mechanism was set

So if:

* SSH session closed 
* App crashed 
* System restarted 

The app would go down silently

---

## Step 5: Permanent Fix is to introduce systemd

To make the application reliable had to move to a **process management approach**.

---

### Create systemd Service

```bash
sudo nano /etc/systemd/system/flask-app.service
```

Add configuration:

```ini
[Unit]
Description=Flask App
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user
ExecStart=/usr/bin/python3 /home/ec2-user/app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

---

### Start and Enable Service

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl start flask-app
sudo systemctl enable flask-app
```

---

### Verify Service Status

```bash
sudo systemctl status flask-app
```
---

## Step 6: Test Reliability

Killed the app process intentionally:

```bash
pkill -f app.py
```

### So, systemd automatically restarted the app

The application was now stable and would restart if stops, in case.
But one last challenge remained…. what if the network itself becomes the problem?
For that we will go onto Incident-4
