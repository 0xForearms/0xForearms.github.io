## eLearnSecurity Certified Professional Penetration Tester Review  

# Background  

eLearnSecurity/INE Certified Professional Penetration Tester(eCPPT) is their professional level course for penetration testing. This course/exam covers all the common topics of penetration testing and offers a good overview of the field. Similar to what I said in my eJPT review, I don’t believe this course is enough to land a pentesting job, as this course is just not as well known as some other certifications in the field. Putting that aside, I feel like this course and exam benefited me greatly in terms of knowledge regarding all areas of penetration testing.  

Similar to the eJPT, one must come into this course with a number of prerequisites:  
  - Understanding a letter of engagement and the basics related to a penetration testing engagement
  - Deep understanding of network concepts
  - Manual exploitation of Windows and Linux targets
  - Performing vulnerability assessment of networks
  - Using Metasploit for complex and multi-step exploitation of different systems and OS's
  - Web application Manual exploitation
  - Ability in performing post-exploitation techniques
  - Exploit development skills on x86 environment
  - Outstanding reporting skills
  
Lets talk about these prerequisites shall we? I went into this course with the knowledge from the eJPT and some additional Hack The Box/Try Hack Me rooms.  I would say I had a good understanding of all of those topics, except the exploit development skill.  That was one that I learned from the course itself as well as some extra non-course work.  Looking at Tib3ruis' [Buffer Overflow Prep room on Try Hack Me](https://tryhackme.com/room/bufferoverflowprep).  Besides that, the course taught me everything I needed to pass the exam.

So what does this course entail:
  - Penetration testing processes and methodologies, against Windows and Linux targets
  - Vulnerability Assessments of Networks
  - Vulnerability Assessments of Web Applications
  - Advanced Exploitation with Metasploit
  - Performing Attacks in Pivoting
  - Web application Manual exploitation
  - Information Gathering and Reconnaissance
  - Scanning and Profiling the target
  - Privilege escalation and Persistence
  - Exploit Development
  - Advanced Reporting skills and Remediation
  
But what does this course actually entail?  I think the PTP course offers a good grasp at what a real penetration test looks like.  It teaches you everything you need to know, from the scoping of the test, performing the test itself, and then the reporting aspect after the test is over.  Something you don't get from their entry level course, the eJPT.

# Labs  
Similar to my eJPT lab experience, I was able to do all the labs with minimal issues.  There were a couple issues with some labs and version mismatches between software, but after short google searches, I was able to resolve them all.  I figured I'd run into issues in a real world assessment so I don't want to knock this.  I ran through the labs twice while they were on the HERA platform, and then once more after they moved to INE to just get a feel for the platform. In short, the INE platform functions very similar to the HERA labs.  The UI is a little less friendly, but it works well enough for the labs.

# Exam  
Unlike the eJPT lab, this one was a full on mock pentest.  You get a letter of engagement with the scope and other information, then you have 7 days of access to the exam network, then an additional 7 days to write up the report.  I see everyone breaking their exam down by days, so I'm going to do that as well.  I took the exam back in November, so bear with me, the details might be fuzzy.

### Day 1  
Kicked off the exam at 0900 or so and read the letter of engagement.  A few minutes later I started the initial network scan and started working on what I discovered.  Found some avenues of attack so I dug into them.  I don't have the exact time frame, but I know I gained access to my first system just before lunch.  After my lunch break, I quickly escalated up to root/system.  Without too many details, I was able to gain access to my second and eventually third system before I broke for dinner/rest.

### Day 2  
Kicked off this day with access to a few systems, but I wanted more.  I don't have many time stamps from this day, but I know by mid morning I had completed the buffer overflow machine(pretty sure it's public knowledge this exists).  After digging around for a bit, I gained access to another system in the afternoon.  Unfortunately this was just a low level user and I was getting stuck.  I decided to break for dinner and some rest.  Figured I had 7 total days and it was just day 2.

### Day 3
I woke up with a new idea on how to priv esc.  I had an idea of what to do, I just couldn't get it to work last night, so I tried my new idea, and it worked.  I gained root/system access to this machine, which was also the overall goal of the test, as defined in the letter of engagement.  I spent the rest of the day writing the report, ensuring I had everything I needed and submitted it before bed.

I received the passing email roughly 20 business days after I submitted it.  They said up to 30, and I was in no rush for the certification so this didn't bother me any.

# Conclusion

eLearnSecurity states their eCPPT exam is a 100% practical and highly respected Ethical Hacking and Penetration Testing Professional certification.  While that might be true, if you search for that keyword on Indeed, you'll get 26 results.. Combine this with the Covid pandemic I wouldn't expect this alone to get you a pentest job, but it certainly makes me feel more comfortable about going into a pentest job interview.
