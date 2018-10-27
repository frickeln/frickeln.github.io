---
layout: post
title: Use Midi Device to Control Faders in Elation Onyx through OSC using oscii-bot
---

I am using [oscii-bot](https://www.cockos.com/oscii-bot) to forward midi commands to Elation Onyx. This way faders, playbacks, etc. can be controll from any midi controller.


Install the Microsoft Loopback Adapter
-------------------

Onyx blocks the use of the local loopback ip (127.0.0.1). To get around that limitation we install the Microsoft Loopback Adapter.
Look [here](https://social.technet.microsoft.com/Forums/windows/en-US/259c7ef2-3770-4212-8fca-c58936979851/how-to-install-microsoft-loopback-adapter?forum=w7itpronetworking) for instructions on how to install the loopbac adapter.

Configure the Loopback Adapter
---------------------
Configure the loopback adapter to use any static ip you want. I used 192.168.44.44 because according to some other Onyx users using an ip in the 192.168 range works best. I have not tested ips outside the 192.168 range, they might very well work.
