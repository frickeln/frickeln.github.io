---
layout: post
title: Control Onyx with BCF2000 using oscii-bot
---
![fader animation](https://raw.githubusercontent.com/frickeln/frickeln.github.io/master/_posts/fader_bcf.gif)

### What can be controlled?
With this script you can control the first 8 main playback faders, their flash buttons and select buttons.
Additionally you can switch playback banks and control the rate belts.
![bcf_config](https://raw.githubusercontent.com/arneboe/con-trol-oscii-bot/bcf2000/bcf_onyx.png)

### Setup
1. First setup osc in Onyx (see [here](https://frickeln.github.io/onyx-oscii-bot-midi-input/)).
2. Download my bcf2000 script from [here](https://github.com/arneboe/con-trol-oscii-bot/blob/bcf2000/BCF2000_to_onyx.txt).
3. Install the script. Don't forget to change the ip inside the script to your loopback adapter's ip.
4. The script contains hardcoded midi channels for all faders. You have to configure your BCF2000 to send those values. The following Image shows how the BCF should be configured:
![bcf_config](https://raw.githubusercontent.com/arneboe/con-trol-oscii-bot/bcf2000/bcf_midi_settings.png)
5. Start oscii-bot
6. Now it should work.

### How does it work?
The script connects to the BCF using midi and to Onyx using osc.
It listens to messages from the BCF, translates them into osc and forwards them to Onyx.
Additionally it listens to osc messages coming from Onyx and translates them into midi messages to control the motor faders of the BCF. I grabbed the osc messages from the official touchOSC file provided by Elation, thus as long as they support touchOSC this script should work.

```
@input osc_in OSC "192.168.44.44:9000"
@output osc_out OSC "192.168.44.44:8000" 1024 0
@input midi_in MIDI "BCF2000"
@output midi_out MIDI "BCF2000"
```


All midi values are hardcoded at the beginning of the script in the configuration section.
```
fader_midi_control_numbers[0] = 1;
...
fader_midi_control_numbers[7] = 8;

downdown_button_midi_control_numbers[0] = 9;
...
downdown_button_midi_control_numbers[7] = 16;

down_button_midi_control_numbers[0] = 17;
...
down_button_midi_control_numbers[7] = 24;

next_button_midi_control_number = 25;
prev_button_midi_control_number = 26;

effect_belt_midi_control_numbers[0] = 27;
...
effect_belt_midi_control_numbers[3] = 30;
```
If you do not want to change the configuration of your BCF you can change the values here instead.

The script distinguishes the different button and fader groups using the midi channel.
Faders are on `ch 1`, flash buttons on `ch 2`, select buttons on `ch 3`, bank select buttons on 'ch 4' and belts on `ch 5`.
Those channels are hardcoded as well. If you change the channels you have to change the constants in the `if` expressions.
`0xB0` means CC message on `ch 1`, `0xB1` CC message on `ch 2`, `0xB2` on `ch 3`, etc...

```//Faders
msg1 == 0xB0? (
//fader code here
);

//Buttons
//down down buttons (on ch 2)
msg1 == 0xB1? (
 // flash button code here
);

//down buttons (on ch 3)
msg1 == 0xB2? (
//select button code here
);

//bank select (on ch 4)
msg1 == 0xB3? (
// bank handling code here
);

//Belts (on ch 5)
msg1 == 0xB4? (
// beld handling code here
);
```


### Contribution
Feel free to change the script in any way you want. If you want to contribute bugfixes or improvements please do so using pull requests on the bcf2000 branch [here](https://github.com/arneboe/con-trol-oscii-bot/tree/bcf2000)

### Issues
Use the issue tracker over [here](https://github.com/arneboe/con-trol-oscii-bot/issues) for any issues, requests and bug reports. Please understand that I am writing this script in my free time. I will try to fix any bugs but it might take some time. 
