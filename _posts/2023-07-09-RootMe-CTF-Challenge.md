---
layout: post
title: RootMe CTF Challenge
author: Melih Eyuboglu
---

This blog is about the solution for the [RootMe CTF](https://tryhackme.com/room/rrootme) Challenge on the TryHackMe platform.

## <span style="color:green;">Task 1</span> Deploy the Machine
-----

Deploy the machine

__*No answer needed*__

## <span style="color:green;">Task 2</span> Reconnaissance
-----

### The Questions 


__Q1:__ Scan the machine, how many ports are open?

| ![NmapScan](\images\RootMeImages\nmapscan.png) |
|:--:|
|*2.1*|

__*The Answer is 2*__

<br>

__Q2:__ What version of Apache is running?

With the help of the Nmap scan above, we can infer that the version is 2.4.29.

__*The Answer is 2.4.29*__

<br>

__Q3:__ What service is running on port 22?

With the help of the Nmap scan above again;

__*The Answer is ssh*__

<br>

__Q4:__ Find directories on the web server using the GoBuster tool.

__*No answer needed*__

<br>

__Q5:__ What is the hidden directory?

I am gonna use the medium list.

| ![Gobusterscan](\images\RootMeImages\gobusterscan.png) |
|:--:|
|*2.2*|

You can see that there is a hidden directory here, which appears to be a control panel.

| ![Panel](\images\RootMeImages\panel.png) |
|:--:|
|*2.3*|

The screenshot of the panel directory looks like this.

__*The Answer is /panel/*__

## <span style="color:green;">Task 3</span> Getting a shell

Find a form to upload and get a reverse shell, and find the flag.

__Q1:__ user.txt

| ![Phpshell](\images\RootMeImages\copyphpreverseshell.png) |
|:--:|
|*3.1*|

As seen in image 3.1, I am copying my PHP reverse shell file to the working directory.

| ![ChangeThis](\images\RootMeImages\changethis.png) |
|:--:|
|*3.2*|

We should modify the IP and PORT parameters in the PHP reverse shell file for our own use. As shown in the image 3.2 above.

| ![Phpuploaderror](\images\RootMeImages\phpuploaderror.png) |
|:--:|
|*3.3*|

When I try to upload the PHP file, it returns an error. Therefore, I am conducting an internet search for 'PHP upload bypass'. 

I have conducted research, and as a result, I am trying to change the file format to the following formats: '.php, .php2, .php3, .php4, .php5, .php6, .php7, .phps, .phps, .pht, .phtm, .phtml, .pgif, .shtml, .htaccess, .phar, .inc, .hphp, .ctp, .module' and then attempting to upload it again.

| ![Phppayloads](\images\RootMeImages\phppayloads.png) |
|:--:|
|*3.4*|

Those are my latest payloads. I have observed that the 'phtml' extension works. 

After uploading the 'phtml' formatted file, I start listening from one of my terminals using netcat. (Here, you need to enter the port that you previously changed in the PHP file.)

| ![netcat](\images\RootMeImages\netcat.png) |
|:--:|
|*3.5*|

Next, it's time to execute the uploaded file. As seen in image 2.2, we have an 'uploads' directory.

We navigate to this directory and execute the uploaded file.

| ![uploads](\images\RootMeImages\uploads.png) |
|:--:|
|*3.6*|

And yes, we are in.

I run a simple find command to locate the 'user.txt' file and read its content with 'cat' command. 

| ![findcommand](\images\RootMeImages\findcommand.png) |
|:--:|
|*3.7*|

__*The Answer is THM{y0u_g0t_a_sh3ll}*__

## <span style="color:green;">Task 4</span> Privilege escalation
-----

__Q1:__ Search for files with SUID permission, which file is weird?

I am gonna use the hint command to answer this question. 

| ![finderperm](\images\RootMeImages\findperm.png) |
|:--:|
|*4.1*|

As you can see, we can use the pythons SUID binary.

__*The Answer is /usr/bin/python*__

__Q2:__ Find a form to escalate your privileges.

We can use GTFOBins.

__*No answer needed*__

__Q3:__ root.txt

| ![suidbinary](\images\RootMeImages\suidbinary.png) |
|:--:|
|*4.2*|

Image 4.2 shows us how to elevate privileges using a Python SUID binary. Before that we should change directory to /usr/bin.

| ![privesc](\images\RootMeImages\privesc.png) |
|:--:|
|*4.3*|

After that you can easily run the find command again like before we did and then you can get the content of the file root.txt

__*The Answer is THM{pr1v1l3g3_3sc4l4t10n}*__