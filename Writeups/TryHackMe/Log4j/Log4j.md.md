https://tryhackme.com/room/solar



###############################
			Cosmin Tibuleac | 14.12.2021
###############################


## Task 1: CVE-2021-44228 Introduction
On December 9th, 2021, the world was made aware of a new vulnerability identified as CVE-2021-44228, affecting the Java logging package `**log4j**`. This vulnerability earned a severity score of 10.0 (the most critical designation) and offers remote code trivial remote code execution on hosts engaging with software that utilizes this `**log4j**` version. This attack has been dubbed "Log4Shell"

Today, `**log4j**` version `**2.15.0rc2**` is available and patches this vulnerability. However, the sheer danger of this vulnerability is due to how ubiquitous the logging package is. Millions of applications as well as software providers use this package as a dependency in their own code. While you may be able to patch your own codebase using `**log4j**`, other vendors and manufacturers will still need to push their own security updates downstream. Many security researchers have likened this vulnerability to that of [Shellshock](https://en.wikipedia.org/wiki/Shell_shock) by the nature of its enormous attack surface. We will see this vulnerability for years to come.

For a growing community-supported list of software and services vulnerable to CVE-2021-44228, check out this GitHub repository:

-   [https://github.com/YfryTchsGD/Log4jAttackSurface](https://github.com/YfryTchsGD/Log4jAttackSurface)

This room will showcase how you can test for, exploit, and mitigate this vulnerability within Log4j.

While there are a number of other articles, blogs, resources and learning material surrounding CVE-2021-44228, I (the author of this exercise) am particularly partial to these: 

-   [https://www.huntress.com/blog/rapid-response-critical-rce-vulnerability-is-affecting-java](https://www.huntress.com/blog/rapid-response-critical-rce-vulnerability-is-affecting-java)
-   [https://log4shell.huntress.com/](https://log4shell.huntress.com/)
-   [https://www.youtube.com/watch?v=7qoPDq41xhQ](https://www.youtube.com/watch?v=7qoPDq41xhQ)[](https://www.youtube.com/watch?v=7qoPDq41xhQ)

**Note from the author:**

**Please use the information you learn in this room to** **_better_ the security landscape. Test systems you own, apply patches and mitigations where appropriate, and help the whole industry recover. This is a very current and real-world threat -- whether you are a penetration tester, red teamer, incident responder, security analyst, blue team member, or what have you -- this exercise is to help you and the world understand and gain awareness on this widespread vulnerability. It should not be used for exploitative gain or self-serving financial incentive (I'm looking at you, beg bounty hunters)**

**Additionally, please bear in mind that the developers of the log4j package work on the open source project as a labor of love and passion. They are volunteer developers that maintain their project in their spare time. There should be absolutely no bashing, shame, or malice towards those individuals. As with all things, please further your knowledge so you can be a pedestal and pillar for the information security community. Educate, share, and _help_.**

## Task 2: Reconnassaince


### Question2: **What service is running on port 8983? (Just the name of the software)**

A: `Apache Solr`

![[Pasted image 20211214020045.png]]
## Nmap: 

*nmap -sC -sV -Pn -T4 -p- 10.10.110.151 | tee nmap.txt*


## Task 3: Discovery

### Question 1: Take a close look at the first page visible when navigating to [`**http://10.10.110.151:8983**`](http://10.10.110.151:8983/). You should be able to see clear indicators that log4j is in use within the application for logging activity. **What is the `-Dsolr.log.dir` argument set to, displayed on the front page?**

A: `/var/solr/logs`



### Question 2: Download the attached files (accessible in the top-right of this task) see some _example_ log files within Solr. Explore each file, if just to get a feel for what is displayed in what log. 

One file has a significant number of `INFO` entries showing repeated requests to one specific URL endpoint. **Which file includes contains this repeated entry? (Just the filename itself, no path needed)**


A: `solr.log`

### Question 3: **What "path" or URL endpoint is indicated in these repeated entries?**


If we take a look at the above mentioned log file (solr.log), we can see that the path that keeps repeating is **/admin/cores**:

![[Pasted image 20211214021926.png]]

A: `/admin/cores`


### Question 4: **Viewing these log entries, what field name indicates some data entrypoint that you as a user could control? (Just the field name)**


