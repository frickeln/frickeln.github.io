---
layout: post
title: Control Onyx with BCF2000 using oscii-bot
---
![fader animation](https://raw.githubusercontent.com/frickeln/frickeln.github.io/master/_posts/fader_bcf.gif)

I made an oscii-bot script that lets you control the main playback faders in Onyx using a BCF2000.

### What can be controlled?
With this script you can control the first 8 main playback faders, their flash buttons and select buttons.
Additionally you can switch playback banks and control the rate belts.


### Setup
1. First follow setup osc in Onyx (see [here](https://frickeln.github.io/onyx-oscii-bot-midi-input/)).
2. Download my bcf2000 script from [here](https://github.com/arneboe/con-trol-oscii-bot/blob/bcf2000/BCF2000_to_onyx.txt).
3. Install the script.
4. The script contains hardcoded midi channels for all faders. You have to configure your BCF2000 to send those values. The following Image shows how the BCF should be configured:
![bcf_config](https://raw.githubusercontent.com/arneboe/con-trol-oscii-bot/bcf2000/bcf_midi_settings.png)
5. Now start oscii-bot
6. Now it should work. Following functions are available:
![bcf_config](https://raw.githubusercontent.com/arneboe/con-trol-oscii-bot/bcf2000/bcf_onyx.png)
