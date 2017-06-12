# dynamic_range_compression
Pulseaudio dynamic range compression on Ubuntu 16.04

# Dynamic Range Compression Notes:

This configures the pulseaudio `pavucontrol` to use the "Steve Harris" plugins to create a dynamic range compressor. This solves the problem of quiet dialog and loud explosions in movies. I turn this on for movies, and off for music. I happen to use a HK Onyx bluetooth speaker.

Packages:

1. pavucontrol - PulseAudio Volume Control
2. swh-plugins - Steve Harris's LADSPA plugins

Install both with `apt-get`.

Edit the default pulseaudio configuration: `/etc/pulse/default.pa` - with this addition to the end of the file. Reboot your laptop.

```
.ifexists module-ladspa-sink.so
.nofail
load-module module-ladspa-sink sink_name=compressor plugin=sc4m_1916 label=sc4m control=1,1.5,401,-30,20,5,12
.fail
.endif
```

Settings for the `control` are:


> RMS/peak: The balance between the RMS and peak envelope followers.RMS is generally better for subtle, musical compression and peak is better for heavier, fast compression and percussion.
Attack time (ms): The attack time in milliseconds.
Release time (ms): The release time in milliseconds.
Threshold level (dB): The point at which the compressor will start to kick in.
Ratio (1:n): The gain reduction ratio used when the signal level exceeds the threshold.
Knee radius (dB): The distance from the threshold where the knee curve starts.
Makeup gain (dB): Controls the gain of the makeup input signal in dB's.
Amplitude (dB): The level of the input signal, in decibels.
Gain reduction (dB): The degree of gain reduction applied to the input signal, in decibels.

References:
 * https://askubuntu.com/questions/31580/is-there-a-way-of-leveling-compressing-the-sound-system-wide
 * http://plugin.org.uk/ladspa-swh/docs/ladspa-swh.html#tth_sEc2.92

## Usage

Once installed there will be a LADSPA audio option available in "Sound":

![sound](sound.png)

If you select the LADSPA as the output source you can see the compression effect in the PulseAudio volume contolr:

![pulse](pulse_volume.png)
