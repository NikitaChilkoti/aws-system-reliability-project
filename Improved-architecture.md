## Improvements and the final architecture

After multiple incidents of application downtime, high CPU usage, and network misconfiguration. The system was finally stable. But stability alone isn’t enough, right? 
A working system is good. A reliable system is what teams actually need.

---

Now, the focus is to improve system reliability, reduce chances of repeated failures, and introduce better monitoring and operational practices

---

## Improvements Implemented

Introduced Process Management (systemd)

Now the application is

* Managed via systemd
* Auto-restarting on failure
* Starting itself on system boot


### Proactive Monitoring with CloudWatch

* CPU utilization alarms
* Status check alarms
* SNS email notifications

> Now, that issues are detected early, the response time has become faster.

---

### Structured troubleshooting approach

Developed a consistent debugging flow:

1. Check EC2 status
2. Check application
3. Test locally (inside server)
4. Check network (SG + NACL)

---

### Incident Documentation

Each issue was documented with:

* Symptoms
* Investigation steps
* Root cause
* Fix

---

## Final Architecture Overview

```id="j8t2mv"
User → Internet → EC2 (Flask App via systemd)
                      ↓
                CloudWatch (metrics + alarms)
                      ↓
                   SNS (email alerts)
```

---

## 📊 System Behavior Now

| Scenario         | Behavior                            |
| ---------------- | ----------------------------------- |
| App crashes      | Auto-restarts via systemd           |
| CPU spikes       | CloudWatch alert triggered          |
| Instance failure | Status alarm triggered              |
| Network issue    | Diagnosable via structured approach |

---

## There can be future improvements that will make this system just right.

Load Balancer
Auto Scaling Group
Centralized Logging
Infrastructure as Code
Monitoring Dashboard

> I believe the goal is to handle the issues better every time.

---

## My final note on this project

This project was all about:

* Understanding failures
* Responding to incidents
* Improving system reliability

I actually witnesses this project from “it works" to “it keeps working”. And I'm super happy about this.

And that’s what we need, right? All of these learnings, boost us in so many ways.

---

## What is deonstrated through this project?

* AWS infrastructure handling
* Monitoring & alerting
* Linux troubleshooting
* Networking fundamentals
* DevOps & SRE mindset

---

> If something breaks now...at least I and you know, where to start.
