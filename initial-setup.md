## Bringing the Application to Life 

I had a simple goal:

> “Let’s deploy our application on AWS and make it accessible to users.”

Simple in theory.

As a Cloud Engineer, my responsibility was to set up the infrastructure and get the application running.

---

## Step 1: Launching the EC2 Instance

Started by provisioning a virtual server using AWS EC2.

### Configuration:

* **Instance Type:** t2.micro (because budget matters duhhh!)
* **OS:** Amazon Linux
* **Key Pair:** Created for secure SSH access
* **Network:** Default VPC (keeping it simple for now)

---

## Step 2: Configuring Security Group

To allow users to access the application:

| Type | Port | Purpose      |
| ---- | ---- | ------------ |
| SSH  | 22   | Remote login |
| HTTP | 80   | Web access   |


---

## Step 3: Connecting to the Server

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

---

## Step 4: Installing Dependencies

Prepared the server to run our application.

```bash
sudo yum update -y
sudo yum install python3 -y
pip3 install flask
```

---

## Step 5: Deploying a Simple Flask App

I created a minimal application:

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "We're Live!"

app.run(host="0.0.0.0", port=80)
```

Ran the app:

```bash
sudo python3 app.py
```

---

### Finally!
Application successfully deployed on EC2 and was accessible via public IP in browser

> That moment when the browser finally loads your app…
> Pure happiness. No CloudWatch alarm needed for that.

---

## What’s Next?

Now that the application is live, the next challenge is:

> “How to know when things break… before users tell us?”

For that I setup monitoring with CloudWatch.
