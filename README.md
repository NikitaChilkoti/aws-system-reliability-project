# The Story behind this Project

This project didn’t start as a “project”
Yeah, I'm talking sound. I started it with a simple setup:

* One EC2 instance and a small Flask application

Everything was working as expected

The application loaded fine in the browser.
There were no errors. No alerts.

So naturally, you know, it felt done.

Until a few hours later, when the application stopped loading.

The tiny bit of confidence I had finally built... packed its bags and well... left the chat.
I quickly checked the instance, and it was still running.
Even the application process was still active.
Nothing looked broken.

But the system wasn’t working.

That was the first sign that:

> “Working” and “reliable” are not the same thing.

From there, issues started unfolding:

* A port wasn’t open, so the app was running but unreachable
* A background process pushed CPU to 100%, slowing everything down
* The application stopped because nothing was managing it
* A network rule blocked traffic in a way that wasn’t immediately obvious

Each issue looked small on its own.

This repository documents that journey:

* How each issue was identified
* How it was debugged step by step
* What the actual root cause was
* And what was changed so it wouldn’t happen again

Over time, the system changed from:

* A setup that worked under ideal conditions
                    to 
* A system that could handle failure, recover, and scale

If you’ve ever looked at a system and thought:

> “Everything seems fine… so why isn’t it working?”

I would walk you in this project with exactly that experience. Hope you'll enjoy the learning. 
