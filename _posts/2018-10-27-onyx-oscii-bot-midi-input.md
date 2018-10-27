---
layout: post
title: Use Midi Device to Control Faders in Elation Onyx through OSC using oscii-bot
---
![fader animation](https://raw.githubusercontent.com/frickeln/frickeln.github.io/master/_posts/fader.gif)


Use [oscii-bot](https://www.cockos.com/oscii-bot) to forward midi commands to Elation Onyx. This way faders, playbacks, etc. can be controlled from any midi controller.


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
Use the following oscii-bot script to test if your Onyx setup is correct.
Don't forget to adapt the ip if you are not using 192.168.44.44.

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
[Download File](https://github.com/arneboe/con-trol-oscii-bot/blob/master/test.txt)

If everything works you should now see the first fader moving up and down and should also see some debug messages in the oscii-bot window.

Example Script
--------------------------
The following example script uses 8 midi faders to control the first 8 faders in Onyx and 8 midi buttons to control the corresponding flash buttons.

```
@input osc_in OSC "192.168.44.44:9000"
@output osc_out OSC "192.168.44.44:8000"
@input midi_in MIDI "con.trol red 2"

@init
midi_channel_to_fader_num = 1024;
midi_channel_to_button_num = 2048;

//the following array defines the mapping between midi fader channel (CC) and mpc fader number.
//NOTE: The fader numbers are zero-based. I.e. the first fader has number 0.
//E.g. if you have a fader that sends on midi channel 7 and want to control fader 3 in mpc
// you would set  midi_channel_to_fader_num[7] = 0;
midi_channel_to_fader_num[7] = 0;
midi_channel_to_fader_num[6] = 1;
midi_channel_to_fader_num[5] = 2;
midi_channel_to_fader_num[4] = 3;
midi_channel_to_fader_num[3] = 4;
midi_channel_to_fader_num[2] = 5;
midi_channel_to_fader_num[11] = 6;
midi_channel_to_fader_num[10] = 7;
//midi_channel_to_fader_num[10] = 8;
//midi_channel_to_fader_num[10] = 9;


//the following array defines the mappin between buttons (note on/off) and mpc flash buttons.
//NOTE: The button mpc button numbers are zero-based.
midi_channel_to_button_num[7] = 0;
midi_channel_to_button_num[6] = 1;
midi_channel_to_button_num[5] = 2;
midi_channel_to_button_num[4] = 3;
midi_channel_to_button_num[3] = 4;
midi_channel_to_button_num[2] = 5;
midi_channel_to_button_num[11] = 6;
midi_channel_to_button_num[10] = 7;
//midi_channel_to_button_num[7] = 8;
//midi_channel_to_button_num[7] = 9;

@oscmsg
//printf("got osc: %s\n", oscstr);


@midimsg 
//Faders (CC)
msg1 == 0xB0? (
	osc_fader_number = midi_channel_to_fader_num[msg2] * 10 + 4203;
	oscsend(osc_out, "i/Mx/fader/%d", msg3, osc_fader_number);
);

//flash buttons (note on)
msg1 == 0x90? (
	osc_button_number = midi_channel_to_button_num[msg2] * 10 + 4205;
	oscsend(osc_out, "i/Mx/button/%d", 1, osc_button_number);
);

//flash buttons (note off)
msg1 == 0x80? (
	osc_button_number = midi_channel_to_button_num[msg2] * 10 + 4205;
	oscsend(osc_out, "i/Mx/button/%d", 0, osc_button_number);
);
```
[Download File](https://github.com/arneboe/con-trol-oscii-bot/blob/master/simple_faders.txt)

