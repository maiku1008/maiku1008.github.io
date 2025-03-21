# My experience passing CKA and CKAD


In December last year I was able to sit for the coveted Certified Kubernetes Administrator and Certified Kubernetes Application Developer exams, and clear them both after some preparation.

This post is meant to share my exam experience, and all the materials used.

### TL;DR
- Read the exam questions carefully.
- Know imperative commands with `kubectl` like the back of your hand.
- Get comfortable looking things up on [kubernetes.io](https://kubernetes.io/).
- Build a bookmark folder in Chrome with relevant pages in kubernetes.io: you can use it during the exam!
- Seriously, read the damn questions carefully.
- Practice, practice and practice more.

### The exams
The exam for any of these Kubernetes certifications consists in solving a set of tasks, in a 2 hour terminal session.

There are 17 scenarios/questions for CKA, and 19 for CKAD.
The whole time, a proctor is watching and listening to you via webcam and microphone, and seeing the content of your screens (you can use more than one).
You communicate with the proctor only via a live chat application which is part of the PSI interface for the exam.

At the beginning they will ask you to show your ID, and then you will need to show them your working area from the webcam.
You will need to show your desk, and the room you're in.
Ensure that nothing written is visibile on the walls, and that you are the only person in the room.

Once the exam starts, the proctor may ask you every once in a while to show your hands to the webcam.
Don't bother asking questions about the exam itself, chances are the proctor really doesn't know about the subject and is only there to ensure you don't cheat.
You will need to share your screens for the proctor to see.
If you have 2 screens you can have the exam terminal on one, and the tab with k8s docs on the other.

In the terminal itself, you are given 6 or so k8s clusters, and you will need to switch between them.
You will always have a command printed at the beginning of each question, for easy copy paste:
```
kubectl config use-context bla
```
Make sure to **use it**!

Do not be fooled by the fact that you are able to search documentation during the exam.
The tasks that you will be given are often complex ones; time is of the essence! If you waste it while reading up docs you will not be able to complete all of the tasks in time.

### Resources used
- Official docs [kubernetes.io](https://kubernetes.io/)
- [Certified Kubernetes Administrator by Mumshad Mannambeth (Udemy)](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/)
- [Certified Kubernetes Application Developer by Mumshad Mannambeth (Udemy)](https://www.udemy.com/course/certified-kubernetes-application-developer/)
- Exam simulator [killer.sh](https://killer.sh/)
- Kodekloud community slack
- Kubernetes community slack

### Extra reading
- [Cloud Native Devops with Kubernetes](https://www.amazon.com/Cloud-Native-DevOps-Kubernetes-Applications/dp/1492040762/ref=sr_1_1?crid=24TO6CY3JORE4&dchild=1&keywords=cloud+native+devops+with+kubernetes&qid=1612074068&sprefix=cloud+native+dev%2Caps%2C260&sr=8-1), by John Arundel and Justin Domingus.
- [Kubernetes Up & Running](https://www.amazon.com/Kubernetes-Running-Dive-Future-Infrastructure/dp/1492046531/ref=pd_bxgy_img_2/140-4642137-3991249?_encoding=UTF8&pd_rd_i=1492046531&pd_rd_r=9b0a86b5-7a7a-48b7-af2d-8caf1db3aeef&pd_rd_w=8ickO&pd_rd_wg=F4zT9&pf_rd_p=f325d01c-4658-4593-be83-3e12ca663f0e&pf_rd_r=GERZBQPX5AYFFCB14S8R&psc=1&refRID=GERZBQPX5AYFFCB14S8R), by Brendan Burns, Joe Beda and Kelsey Hightower.

## CKA
I will start with the CKA, as it is the first exam I attempted.

### Objectives
The CKA ensures we have the skillset to setup and maintain a Kubernetes cluster, and manage it by using best practices.

### Requirements
As this exam is aimed at the Systems administrator/devops engineer, some linux as well as infrastructure skills are expected, such as:
- Knowledge of Linux OS.
- Being comfortable on the terminal.
- Knowing how to quickly edit files with text editors like Vi/Vim.
- Working knowledge of networking concepts.
- Working knowledge of TLS encryption concepts.
- Working knowledge of docker and containerization concepts.

A deep dive is done on Kubernetes concepts and architecture during the indicated courses.

### Study plan
I began studying around the end of September 2020, by purchasing Mumshad Mannambeth's excellent Udemy course for both CKA and CKAD, both on sale at the time.
That was a major source of material for me, as the courses included access to Katacoda/Kodekloud practice labs, and access to a Slack community where the trainer and other exam-takers participated.
The videos of Mumshad's course are very well done, each in the 5-10 minute range in length, slowly and clearly explaining each concept.
I would get to practice that concept later with labs provided by Mumshad's platform, KodeKloud.

The course itself has a section later on where we must manually install a Kubernetes cluster from scratch on a local vagrant setup, based on [Kelsey Hightower's excellent Kubernetes the Hard way project](https://github.com/kelseyhightower/kubernetes-the-hard-way) (the original project leverages infrastructure on Google Cloud instead).
During the same course, we get to setup another cluster by using [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/), which is part of the exam curriculum.

I would say that before taking the exam, I spent an average of about 1 hour every day watching the course videos and playing around with the labs (around 2 hours or more during weekends), for a couple months;
I took care of understanding every concept presented in the videos, and verified my knowledge with the labs and on a local cluster where I spent some practice time on, on top of study time.
I also re-did all of the course labs multiple times until I knew almost all of the answers by heart, and could get around them easily.

When something was unclear, the slack communities on kodekloud and kubernetes were always very helpful, always answering all of my questions, no matter how dumb I thought they were :)

I made sure to know `kubectl` imperative commands like the back of my hand, and building a library of bookmarks in my browser with relevant pages ready, as we are allowed an extra tab with the documentation during the exam.

After 2 months of that, I felt like I was ready.

### CKA Exam attempt #1
I booked the exam for end of November, on a Saturday morning at 8AM, and sat for it in the guest room of my home.
The exam setup time with the proctor went pretty smoothly, and I was able to start on time.

I was not so confident by the end of the two hours. After 36 hours I got the news that I failed that attempt with 60% (minimum passing grade is 66%).
What went wrong for me was that I did not read/understand one specific question, which was related to ETCD backup and restore (part of the official curriculum); because of this, I ended up performing a task that effectively made one of the exam clusters unavailable.

Looking back, I am quite sure I invalidated several correct answers due to this mistake, which resulted in the score.
The method of how to backup/restore an ETCD cluster was also not entirely clear to me at the time, so I was not 100% sure of what I was doing.

Clearly I overestimated my knowledge, and depended too much on memorizing certain commands on one study platform, instead of being completely confident in all concepts and commands applied.

### New study plan
Enter [killer.sh](https://killer.sh/).
A CKA/CKAD/CKS exam simulator created by [Kim Wuestkampf](https://www.linkedin.com/in/kimwuestkamp/).

For CKA, the simulator offered 25 HARD questions to be done in 2 hours, with guided and comprehensive answers made available after the alotted time.
Such answers also offered different ways to do things, for maximized learning experience.

This alternative platform helped a lot in understanding things that weren't clear to me, and to practice more varied scenarios.
Completing a killer.sh simulator in under two hours gives good confidence in being able to tackle the real exam.

### CKA Exam attempt #2
I booked the exam one week after getting the results of the previous one.
The exam setup experience was again pretty smooth and I was up and ready to go 10 minutes before the start time.

I felt pretty confident the whole time and finished up all of the questions around 1 hour and 10 minutes later.

The results came around 36 hours after the end of the exam, in an email.
Score was a pass with 85% :)!
As it is not possible to see the actual answers, to this day I don't really don't know what I got wrong, but am still satisfied with the result.

## CKAD
I took the CKAD exam while the knowledge of the previous training was still fresh, so I felt like I had an easier time with this one.

### Objectives
To be able to efficiently design and implement working kubernetes production workloads.

### Requirements
Not so much system administration knowledge, but more focus on k8s concepts, architecture and best practices.
Actual programming experience is not required for the exam (although in "real life" it will probably not hurt either ;) ).

### Study plan
I booked the CKAD exam roughly for 3 weeks after passing the CKA.
In those 3 weeks, I took my time and spent again around 30min to 1 hour a day on Mumshad's course and labs.

When exam time was closer, I booked two sessions on killer.sh and focused on practicing there.
I challenged myself by finishing all the questions under 2 hours, and afterwards going through slowly through the answers section on the site.
This was very helpful and allowed me to learn different, more effective approaches.

### CKAD Exam attempt #1
I once again took the exam from the guest room in my home.

This time the exam setup with the proctor wasn't so smooth.
The connection from my end and theirs wasn't great, and we kept getting disconnected from the session. Webcam images weren't so clear so they had to ask me to show identification more than once. It took about 40 minutes to start the actual exam, and that did get me a bit stressed out.

Nevertheless, I felt comfortable and finished all the questions in about 1 hour and a half.
36 hours later, I got the email with the good news of a pass with 83%.
Doing the CKA before really helped with the whole experience.


## Conclusion
From my experience, I would say that the hardest exam was the one for CKA, as it didn't just go through the new knowledge of k8s concepts, but also on existing understanding of systems administration.
I felt like doing CKAD immediately afterwards made it a bit easier, as the knowledge was still fresh.

The CKAD questions however were much more time consuming, in the sense that one task would require to perform several smaller tasks quickly, building a workload and making sure everything works together.

All in all, I feel the time trying to tackle these certifications was well spent; since the exams are performance based, they are fair in what they expect from you, and ensure that once the certs are in your hands, you really actually know your stuff. :)

Next on my list is the new Certified Kubernets Security Specialist certfication, for which I will follow a similar approach as the above.
Perhaps a blog post focusing on that will come soon.

