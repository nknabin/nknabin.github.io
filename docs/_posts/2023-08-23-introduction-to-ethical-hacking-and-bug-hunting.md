---
layout: post
title: "Introduction to ethical hacking and bug hunting"
date: 2023-08-22 05:03:12 +0545
categories: security
---

## Introduction
Ethical hacking is the process of challenging online businesses and organizations to various security assessments. It can be in different forms including penetration testing for organizations or businesses, bug bounty hunting, security assessments for organizations and more. It is a continuous process as systems and software are continuously developed and modified. The main reason behind ethical hacking is to find the security loopholes in your systems before a malicious actor so that you patch the loopholes before they are exploited.

## What knowledge or tools do I need beforehand?
You can embark on your journey in ethical lacking with little to no knowledge of computer systems. If you have no prior experience working with any form of software or operating systems, you will need to learn a lot. There are also some classes of bugs or vulnerabilites that don't require knowledge of programming or complex system architectures, which might help to get started in this field. All you need is a willing heart and devotion to spend hours using and understanding the system so you can find areas/features to hack it.

You are ready to start if you had some experience with software development or system administration. With such experience, you probably have some idea about how the Internet works and how to maintain your system. Here are some things that might help you in this field: 

- Linux OS (optional but most tools are written for Linux, although they can be installed on Windows as well)
- How Internet works (TCP/IP, OSI, HTTP requests and responses)
- Basic programming (basic python) (useful for scripting and understanding and running exploits)
- Basic web application development (useful in looking for anomalies in web application source code and develop a sense of possible areas or features for vulnerabilities)

### The most important thing that you need for success

The only thing that you require is a sound discipline for continuously putting in efforts. This means understanding that there might be days in which there won't be any substantial results and you will be easily demotivated, especially if you are doing this as a personal hobby. In such situation, the most crucial factor deciding your success will be if you can stick to your schedule of continuously probing the target for expanding its attack surface and therefore, increasing your chance of finding any bugs or vulnerabilites.

Therefore, it is very crucial to remind yourself why you got into this and keep putting on more hours on understanding the system and its features better. Some hackers have found a bug in a matter of few hours, while some have probed the target for days, weeks, or months. If you are getting tired of a target or an asset, switch to another one for a little while. This will keep your interest from further reclining and will allow for a fresh perspective when you get back to the old target, which might just be what you need.

## Technical terms for real world ethical hacking or bug hunting:
- Recon<br>
It is the process of going through the target to identify its features, assets, services, end points, and more to find potential attack ports and point of entries to the target.

- Asset<br>
It refers to anything that is owned by the target. It can be a website, a sub-domain, or any other hardware. Before running any kind of tool on an asset, make sure it falls under the scope of your operation.

- Scope<br>
A target will always have its scope that lists out the assets that can be attacked. When you discover an asset, always check if it is in scope. Do not lay your hands on something that's out of the scope.

- Attack surface<br>
The attack surface consists of all the assets, features, and areas of the target that fall within the scope of your operation. Greater the attack surface, higher is your chance of finding a vulnerable piece of area in your target.

- Exploit<br>
It refers to a program or a piece of code that successfully takes an advantage of a vulnerability found in an asset to steal some data or do some damage to the target. It can be a whole program, a piece of it, or a custom script that utilizes multiple tools.

- Proof Of Concept (PoC)<br>
This is the most important part of bug hunting or ethical hacking. As the name suggests, it is a document that provides proof of your exploit that the target has really been compromised using the techniques mentioned in the exploit. It can be a screenshot with output that contains confidential data, or text (or other) files extracted from the target containing confidential data, or a video demonstrating how to use the exploit and more.

## Recommended setup
### OS
Linux is recommended. Any flavor of Linux will work. Choose the one that you are most comfortable with.

- If you are new to this, use kali Linux inside VirtualBox.
- If you want to install Linux directly on your host, install Ubuntu.
- If you are using Windows, install kali Linux inside VirtualBox or the WSL.

### Note taking app
This is an essential tool to develop a good methodology and discipline for hunting for bugs and ethical hacking. A good note taking tool and habit will help you efficiently organize and manage your attack plans and reports, and therefore, maximize your chance of success.

Here are some recommended tools for taking notes:

* Windows OS: MicroSoft OneNote
* Linux: CherryTree
* Kali Linux: CherryTree (Installed by default)

 Here are some guidelines for taking note:
1. Do not note down everything as that will bloat your notes very fast.
2. Use a hierarchical note taking tool (like CherryTree). This helps in keeping your note organized.
3. Arrange your notes with priority. Order by the importance of an asset or the impact they can have if they are compromised. Order by how easily can a target be attacked. Go through them in the same order.
4. Include timestamps in each note you create, especially when you add any Proof of Concept (PoC) to your note.
5. Take notes of what you think are interesting and can be exploited. Come back later to this to explore.
6. Back up your notes safely, preferably in cloud. Backup only what you will explicitly need in later date (PoCs and other findings).

### Browser
Firefox is the recommended browser as it contains the best tools and add-ons (plugins) for tweaking with software. However, any browser will work when you are starting. As you learn, you will get accustomed to one.

Recommended plugins for beginners:
- FoxyProxy<br>
It allows you to send the requests from your browser to another app (proxy). This is extremely useful in studying requests made by the target you are scoping for.

- Wappalyzer<br>
It provides a list of the tools that website is currently running. It shows whatever it can identify, including the following:
  - Operating Systems
  - Server
  - Database
  - CMS
  - JavaScript Frameworks
  - Payment Methods

- WhatRuns<br>
It provides the same functionality as Wappalyzer.

### Other Tools
As you keep learning, knowing what your tool does, even behind the scenes, is important. Some tools might breach the code of conduct set by the employer you are hacking for or bug bounty targets and their scopes. Grow accustomed to a particular set of tools. There are no tools superior to another. The tools are just that: tools. Your success depends on how much time you spend understanding the system and how you use the tools to go around them.

An important aspect is the method you use to install the tools. It is highly recommended that you install a tool with the method listed in the tool's official GitHub page or website. You can also install a tool using your package manager, if provided. However, manually installing and updating tools can be beneficial as this ensure latest updates quicker than your package manager.

Here are some tools that are recommended for beginners. You will install more as you keep learning.

- BurpSuite<br>
It allows you to intercept, study, and modify the requests and responses. You will be spending a lot of time in BurpSuite if you are trying to learn web hacking. You will mostly be using the Proxy, Repeater and Intruder tools in burpsuite, which are available in the community edition as well.

- SecLists<br>
A highly useful collection of multiple lists used in security assessments. It contains wordlists that you will be using as you hack more.

- Others<br>
  You will need some other tools as you continue learning more complex concepts. Install these when you need them.
  - crtsh, certspotter
  - httprobe, httpx,
  - amass,
  - ffuf

## Methodology
A good and efficient methodology is your key to success in the field of ethical hacking and bug bounty. It outlines your whole process from scoping a target to finding a bug to reporting the bug. Start with the basic methodology as shown below. As you continue to hack, adapt them to suit the kind of bugs you mostly look for or the kind of hacks that really interest you.

1. Recon
The first step is the reconnaissance. You scope your target and identify its features and assets and map an attack surface and create a plan of attack.

2. Attack
You go through the potential attack ports and try to exploit the system. Remember to check if the asset and the method of your attack falls under the scope. Many targets don't allow some kind of attacks such as DoS.

It is very important to take screenshots or record videos if your attacks are successful, for PoCs. It is also extremely important to timestamp your attack and clearly list the asset, and attack method in your PoCs.

3. Submit your report (or a flag)
The final step is to submit your report. Report writing is very crucial as it directly determines the impact of your work. Make sure to write a clear and concise report that clearly indicates the severity and impact of the vulnerability. A bad report will undermine your work and underplay the bounty that you deserve. It might even let essential information to be excluded from executive decisions.

Follow the outline for the reports that are provided by the company you work for or the bug bounty platforms. Bug bounty platforms like HackerOne provide their own outline when you try to submit a report. Stick to these outlines.

Here is a outline for a good report:

1. Title
2. Description
3. Affected assets
4. Severity (Informational, Low, Medium, High, Critical)
5. Mitigation
6. Reproduction steps
7. Proof of Concept (PoC)
8. Discovery detail

## Understand your target and choose the tools accordingly
While mastering the tools will help you get more efficient, remember that your job is to study the target to understand it better so you can hack it. Tools are just a means for this purpose. First, understand the target and its features. Second, find out a point of attack or a point of entry. Only then, choose your tool wisely for the kind of attack you have planned. Having said this, master a certain set of tools for general purposes (e.g., asset discovery and port scanning).

First, ask yourself questions such as:
- What features are available for a normal user and an admin user?
- Is the system divided between different types of clients?
- Is there a password reset system?
- Is there a real time chat feature?
- Is there a file or photo upload feature and many more?
- Are there input fields that communicate with the database?

After you have a complete understanding of the system of its subset, then you cak ask yourself questions such as:
- Can a normal user access admin functions?
- Can one user view data of another user?
- Can one user reset the password for another user?
- Can a user send messages as another user?
- Can the file upload feature be exploited?
- Is there any indication of SQL injection? Or is there a way to get more information on the database?

Finally, depending on the set of features you plan to attack, choose your tools wisely. Remember that there is no best tool available for any task. Different tools perform differently. Build up your experience with these tools to understand which tool is the best for your task. Also, it is important to get acclimated to a set of tools in order to increase your efficiency.

## Points to remember
1. Discipline<br>
   Always make efforts to maintain a good discipline to maximize your success. The successful ones in this field are the ones with a sound discipline rather than the ones with the best knowledge of the systems and tools.

2. Scope<br>
Always remember to check the scope of the asset or your method of attack. Attacking (or even visiting or looking at data from) an asset that is out of scope may win you a permanent ban from the bug bounty platform or your work and sometimes, even legal issues if proven to be intentional.

3. Note
Make sure to take efficient, concise and clear notes. You may often fing yourself switching your targest. Take your notes in such a way that coming back to a target is easy.

4. Backup
   Perform necessary back up of your main data (notes, PoCs and recon data).

   Also, it is highly useful to back up your system and tools (OS and tools) or create a script to quickly set up your hacking environment in a new machine. Hacking from your own machine is not always advisable and you will find yourself using a hacking environment inside a VPS like Digital Ocean.

5. Understand the target, the tools come later.
While the tools will help you get information about the target, always focus on the target first and its features. Study the features very thoroughly and understand what its system is designed for.
