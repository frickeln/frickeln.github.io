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

Configure Onyx
---------------------
Enabled osc in Onyx. [This](http://support.obsidiancontrol.com/Content/Networking/OSC.htm?Highlight=osc) manual page shows how. 
Make sure that you enabled the loopback adapter and set the port to 8000. Enable a device and use the the ip of the loopback interface as ip (192.168.44.44 in my case). Set the port to 9000.

Setup OSCII-Bot
--------------------
Get it [here](https://www.cockos.com/oscii-bot).
Use the following script to test if your Onyx setup is correct.
Don't forget to adapt the ip if you are not using 192.168.44.44
```
@input osc_in OSC "192.168.44.44:9000"
@output osc_out OSC "192.168.44.44:8000"

@init
i = 0;

@timer
	i >= 255? (
		i = 0;
	) : (
	i = i + 1;
	oscsend(osc_out, "i/Mx/fader/4203", i);
	)

@oscmsg
printf("got osc: %s\n", oscstr);
```
If everything works you should now see the first fader moving up and down and should also see some debug messages in the oscii-bot window.
