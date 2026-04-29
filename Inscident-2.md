## Incident 2 – High CPU Usage

With monitoring actively tracking system performance, the setup was doing its job well. Kind of.

And then… another alert arrived.

---

## Incident Trigger

> **Alarm:** CPUUtilization > 70%
> **State:** ALARM

CloudWatch detected sustained high CPU usage on the EC2 instance.

> This time monitoring caught it early. Exactly what we wanted.

---

## Step 1: Connect to the Instance

```bash id="a8d2kp"
ssh -i your-key.pem ec2-user@<public-ip>
```

---

## Step 2: Check System Load

Used `top` to inspect real-time resource usage:

```bash id="x1m9qw"
top
```

### What did I observe?

* CPU usage was consistently near **100%**
* One process consuming most of the CPU

> The server wasn’t just busy… it was I would say panicking! :D

---

## Step 3: Identify the Problematic Process

From `top`, we noticed:

* A process named `yes` consuming high CPU

> Turns out, this was from our earlier test… and our Vit D

---

## Step 4: Kill the Process

```bash id="z7p4vn"
killall yes
```

---

## Step 5: Verify System Recovery

Ran `top` again:

```bash id="l2s8rf"
top
```

### CPU usage dropped to normal levels

---

## Step 6: CloudWatch Recovery

* Alarm state changed from **ALARM → OK**
* No further alerts received

---

## System performance was restored… but stability was still not guaranteed.

Because soon after, the application stopped responding completely. I'm excited to share Incident 3.
