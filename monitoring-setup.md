## Adding Monitoring setup to know early if things break

The application was now live on EC2 and accessible to users.

Everything looked fine…
which I believe... is usually the most dangerous phase in production.

The team needed visibility into:

* System health
* Performance issues
* Failures before users complain

So, the next step was clear:

> “Let’s set up monitoring and alerts.”

---

## Step 1: Understanding What to Monitor

Focus was on **key EC2 metrics**

* 'CPU Utilization' that detects overload
* 'Status Checks' that detects instance/system failure

> If CPU spikes or instance fails, the company should know *before* users do.

---

## Step 2: Creating CloudWatch Alarm (CPU Utilization)

1. Go to **CloudWatch → Alarms → Create Alarm**
2. Select metric:
   * EC2 → Per-Instance Metrics → CPU Utilization
     
4. Choose your EC2 instance
5. Set condition:
   * Threshold: **Greater than 70%**
     
6. Period: 5 minutes

> Keeping this 70% is safer because IYKYK :)

---

## Step 3: Setting Up SNS Notification

What's the use of creating an alarm with no sound, right? So setting up mail to get a notification if this threshold is reached.

### Create SNS Topic:

1. Go to **SNS → Topics → Create Topic**
2. Type: Standard
3. Name: `cpu-alert-topic`

### Add Subscription 

1. Protocol: Email
2. Enter your Email
3. Confirm subscription from inbox

---

## Step 4: Link Alarm with SNS

While creating the alarm:

* Select **Send notification to SNS topic**
* Choose: `cpu-alert-topic`

---

## Step 5: Testing the Alarm

Simulate high CPU usage:

```bash id="3g8nzk"
yes > /dev/null
```

> This command will eat all the space, that ideally would trigger the Cloudwatch alarm and that would be notified via Email.

---

## Finally, first alert received. And we're in control.

---

## Now stop the Stress Test

```bash id="v6t9q2"
killall yes
```

---

## Step 6: Status Check Alarm

I also created an alarm for instance health:

* Metric: **StatusCheckFailed**
* Condition: ≥ 1

> If this triggers, something is seriously wrong.

---

## So now, 

CloudWatch helps track real-time metrics
SNS enables instant alerting
Monitoring is essential for production systems

---

Now comes the real test

> “What happens when something breaks?”

 Prevention is okay, but we have to consider what's the cure. So, taking one incident at a time- Incident 1
