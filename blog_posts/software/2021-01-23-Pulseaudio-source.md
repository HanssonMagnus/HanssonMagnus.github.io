## Introduction
Audio settings can be a bit confusing on Linux. There are different layers of
the audio configuration, at the bottom you find ALSA which usually directs output to PulseAudio.
ALSA is a kernel subsystem that handles the sound card drivers, it comes with an API and cli
utilities such as amixer. PulseAudio is an audio server that acts as an intermediary between ALSA
and applications.
It is recommended to change audio settings through PulseAudio if that is used, instead of through
ALSA.

## pactl - Control a running PulseAudio sound server
pactl can be used to issue control commands to the PulseAudio sound server. Look also into the man
pages for pacmd, which it used to reconfigure a PulseAudio server during runtime, as well as
pulseaudio.

## Output device
When using multiple output devices it is not intuitive how to choose between them. To list your
alternatives you can run,

```
pactl list short sinks
```
this command will list all currently available sinks in a tabular format which is easy to parse
with scripts,

```
0       alsa_output.usb-Focusrite_Scarlett_2i2_USB-00.analog-stereo     module-alsa-card.c      s32le 2ch 48000Hz       RUNNING
1       alsa_output.usb-Kingston_HyperX_Virtual_Surround_Sound_00000000-00.analog-stereo        module-alsa-card.c      s16le 2ch 48000Hz       SUSPENDED
2       alsa_output.pci-0000_0b_00.3.iec958-stereo      module-alsa-card.c      s16le 2ch 44100Hz   SUSPENDED
```

Now, in the same fashion you can list all sink inputs with,
```
pactl list short sink-inputs
```

```
8       0       493     protocol-native.c       s16le 1ch 44100Hz
9       0       494     protocol-native.c       s16le 1ch 44100Hz
11      0       496     protocol-native.c       s16le 1ch 44100Hz
12      0       497     protocol-native.c       s16le 1ch 44100Hz
16      0       507     protocol-native.c       s16le 2ch 32000Hz
28      0       130     protocol-native.c       float32le 2ch 48000Hz
```

If we want to move a specific playback steam we can do this by,

```
pactl move-sink-input ID SINK
```
E.g.,
```
pactl move-sink-input 28 1
```
would move sink-input 28 to sink 1.

## A note on volume control
In my i3 config I have the following,

```
bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume 0 +1% #increase sound volume
bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume 0 -1% #decrease sound volume
bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute 0 toggle # mute sound
```
Note that this sets the volume for my sink 0, which in my case is a Focusrite Scarlett sound card.

In my i3 panel the default sink is shown and that can be changed with,

```
pacmd set-default-sink 0
```

Although for some reason this does nothing to the audio output.

## A script that changes all current streams
Instead of changing the output manually each time, we can write a script that does it for us. This
is easily done since the output of the pacmd command is in standard tabular form. We can use awk to
select the first column, and then pipe that into a loop moving each audio stream to the new sink.
We can choose the new sink to be 1 if the current is 0 and 0 if the current sink is 1.

```
#!/bin/bash
# Moves the current audio steams to the not curruent sink, which is either 1 or 0.

# If the fist element of the audio streams is at sink 0 then the new sink is 1.
if [ "$(pactl list short sink-inputs | awk '{print $2}' | head -n 1)" == "0" ]; then
    newSink=1
    echo New Sink is: $newSink
else
    newSink=0
    echo New Sink is: $newSink
fi

# Move each stream to $newSink
pactl list short sink-inputs | awk '{print $1}' | while read stream
do
    echo "Moving stream $stream to sink $newSink"
    pactl move-sink-input "$stream" "$newSink"
done
```
