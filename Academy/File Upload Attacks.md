Many modern web applications have file upload capabilities, which are usually necessary for the web application's functionality to enable features like attaching files or changing a user's profile image. If the file upload functionality is not securely coded, it may be abused to upload arbitrary files to the back-end server, eventually leading to compromise of the back-end server.

When an attacker can upload arbitrary files to the back-end server, they can upload malicious files, like web shells, which would enable them to execute arbitrary commands on the back-end server. This eventually allows attackers to take control over the entire server and all web applications hosted on it, which makes `File Upload Attacks` among the most critical web vulnerabilities.

This module will discuss the basics of identifying and exploiting file upload vulnerabilities and identifying and mitigating basic security restrictions in place to reach arbitrary file uploads.

In addition to the above, the `File Upload Attacks` module will teach you the following:

- What are file upload vulnerabilities?
- Examples of code vulnerable to file upload vulnerabilities
- Different types of file upload validations
- Detecting and exploiting basic file upload vulnerabilities
- Bypassing client-side file upload validation
- Bypassing blacklisted and whitelisted extension validation
- Bypassing type and content validation
- Bypassing other basic security restrictions
- Attacking upload forms with limited allowed file types
- Preventing file upload vulnerabilities through secure validation techniques

`CREST CPSA/CRT`-related Sections:

- Intro to File Upload Attacks
- Absent Validation
- Upload Exploitation

`CREST CCT APP`-related Sections:

- All sections

`CREST CCT INF`-related Sections:

- All sections

This module is broken into sections with accompanying hands-on exercises to practice each of the tactics and techniques we cover. The module ends with a practical hands-on skills assessment to gauge your understanding of the various topic areas.

You can start and stop the module at any time and pick up where you left off. There is no time limit or "grading," but you must complete all of the exercises and the skills assessment to receive the maximum number of cubes and have this module marked as complete in any paths you have chosen.

As you work through the module, you will see example commands and command output for the various topics introduced. It is worth reproducing as many of these examples as possible to reinforce further the concepts presented in each section. You can do this in the `PwnBox` provided in the interactive sections or your virtual machine.

The module is classified as "`Medium`" and assumes a working knowledge of the Linux command line and an understanding of information security fundamentals. The module also assumes a basic understanding of web applications and web requests. It will build on this understanding to teach how Arbitrary File Upload vulnerabilities work and how to exploit them.

In addition to the above, a firm grasp of the following modules can be considered as prerequisites for the successful completion of this module:

- Linux Fundamentals
- Web Requests
- Introduction to Web Applications
- Using Web Proxies
- Web Attacks







# Skills Assessment - File Upload Attacks

You are contracted to perform a penetration test for a company's e-commerce web application. The web application is in its early stages, so you will only be testing any file upload forms you can find.

Try to utilize what you learned in this module to understand how the upload form works and how to bypass various validations in place (if any) to gain remote code execution on the back-end server.

## Extra Exercise

Try to note down the main security issues found with the web application and the necessary security measures to mitigate these issues and prevent further exploitation.

---
## Questions

Answer the question(s) below to complete the section

Try to exploit the upload form to read the flag found at the root directory "/".

