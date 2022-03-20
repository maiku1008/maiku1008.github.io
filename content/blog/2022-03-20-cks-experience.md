---
title: "My experience passing Certified Kubernetes Security Specialist (CKS) exam"
date: 2022-03-20T08:42:47+02:00
description: "Tackling the last (and arguably hardest) Kubernetes certification, for fun and for profit"
keywords: [experience, devops, cloud, kubernetes, cncf]
draft: false
tags: [experience, devops, cloud, kubernetes, cncf]
math: false
toc: false
---

Earlier in 2021, right after taking the exams for CKA and CKAD in succession (the experience of which I documented [here](https://blog.cuffaro.com/blog/2021-01-24-cka-ckad/)), I quickly booked the brand-new (at least at the time) CKS exam. Life happened, and I've ended up procrastinating the study and preparation process until now 😅.

Today I got the great news of a PASS, and once again I would like to share my experience in obtaining this tough certification.

### Preparation

I've used a similar preparation process as the previous two certs.

I first went through [Kodekloud's CKS course with labs](https://kodekloud.com/courses/certified-kubernetes-security-specialist-cks/); this course has a great mix of theory (short videos, not more than 15~ minutes long), and practice (actual lab scenarios in browser terminal). I found this to be an easier introduction to the topics. It also has few mock exams to test your knowledge at the end.

I essentially spent around 3-4 weeks going through all the content and labs, and ensuring to have a good understanding of each topic.
I then proceeded to drill the mock exams and do them again and again for about a week or so, until I could do them with my eyes closed 😀.

After this, I switched to [Killer.sh Kubernetes CKS 2022 Complete Course](https://www.udemy.com/course/certified-kubernetes-security-specialist/) on udemy.
I made sure to go through all the labs, and revisit any topic that needed refreshing.

Killer.sh's approach is more thorough than Kodekloud's. Explanations are more in depth, and the trainer likes to show different ways to accomplish tasks, for a better learning experience. The labs are also harder/trickier.

Once I felt comfortable with my knowledge from these two courses, I signed up on [Killer.sh CKS exam simulator](https://killer.sh/cks).

This gives access to 2 36-hour sessions on the Killer.sh platform, where there are about 25 hard scenarios that are extremely similar to real exam scenarios.
Thorough solutions and scores are provided at the end of a two-hour timer.

The environment and timer can be reset as much as you want during the 36 hour time window, giving room to experimentation and more learning experience.

In my first couple tries with the Killer.sh simulator, I got completely and utterly destroyed, and quite honestly, a little bit depressed 😅;
But attempting the whole series of questions again and again in as little time as possible, slowly built my confidence as my score kept getting better.
I did both simulator sessions during the week before the actual exam, while also still revisiting the Kodekloud mock exams, to keep that practice up.

Overall, the preparation effort for this exam equalled to **dedicated study + practice for a couple hours every day for about a month and a half**, with the last week being a little bit more intense focusing on simulators.

### The Exam

I had an issue during pre-exam checks with the proctor;

I was using Chrome and I wasn't able to share with them the view of my monitors: there was something wrong with the Chrome plugin.

The proctor suggested to use Microsoft Edge to connect to the session, so I spent about 10 panicked minutes downloading the app, configuring my bookmarks on it, and ensuring the exam plugin worked.
Luckily it did, and I was able to start about 15 minutes after the actual start time.

The exam itself consisted of about **16 complex scenarios** to be completed in 2 hours.
The pass mark is **67/100**, and partial attempts count towards the final score.

Even though the questions are in my experience less than the ones in previous two certification exams, the available time is barely enough to complete all of them.

I knew this beforehand, so I went ahead and tackled first all the questions with higher score, flagging the easier ones for later.

After 1 hour and a half, I was left with 3 flagged scenarios which I had attempted, but wasn't sure about.
One of them was one of the high score ones, and I spent the last 30 minutes trying to figure out the issue without much success.

I ended the exam at the very last minute, my confidence not being very high; I had a gut feeling that I would need to use my retake attempt, because according to my calculations adding up the various question scores, the total I had barely made the passing mark.

Exactly 24 hours after the exam ended, I got my certificate and exam score of **81/100**, which was quite surprising, but very satisfying 😀.

### Tips

Here's a few tips I can think of, in no particular order, to get ready for sitting for the exam:

- Know imperative commands with `kubectl` like the back of your hand.
- Get comfortable looking things up on [kubernetes.io](https://kubernetes.io/).
- Build a bookmark folder in your browser of choice with relevant pages.
- For this exam, you are allowed to browse online documentation for other relevant tools, like [Trivy](https://aquasecurity.github.io/trivy/) or [Falco](https://falco.org/docs/). Be sure to read your candidate handbook for a complete list.
- You do not need to learn [rego](https://www.openpolicyagent.org/docs/latest/policy-language/) language by heart, but being able to read it to solve some of the Open Policy Agent questions will help you a lot.
- Prepare Chrome, Microsoft Edge and Vivaldi web browsers with the PSI exam plugin, complete with bookmarks, before exam day; just in case you have last minute browser glitches like I had.
- Make sure you really understand how to use sysdig and falco, in case you need to create new alert rules for container syscall activity from scratch.
- Attempt all questions; **correct partial tasks count towards the final score!**
- Practice, practice and practice more; build that muscle memory 😀

### Resources used
- Official docs [kubernetes.io](https://kubernetes.io/)
- [Kodekloud's CKS course with labs](https://kodekloud.com/courses/certified-kubernetes-security-specialist-cks/)
- [Killer.sh Kubernetes CKS 2022 Complete Course](https://www.udemy.com/course/certified-kubernetes-security-specialist/)
- [Killer.sh CKS exam simulator](https://killer.sh/cks)
- Kodekloud community slack
- Killer.sh community slack

### Extra reading
[Container Security](https://www.amazon.com/Container-Security-Fundamental-Containerized-Applications/dp/1492056707/ref=sr_1_3?keywords=container+security&qid=1647773522&sprefix=container+secu%2Caps%2C348&sr=8-3), by Liz Rice. 

Not a must-read, but an excellent one especially if you enjoy the topics covered in the courses.

## Conclusion
The CKS exam is definitely the hardest one I've tackled so far, and the final one in the trio.
As for the other certifications in the series, it is tough, but fair; definitely achieavable with a good study plan and resources, and especially enough discipline to follow through 😀.
