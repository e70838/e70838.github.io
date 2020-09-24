---
title: "linux sound"
date: 2020-09-24
---
L'article est [Clean ALSA documentation](https://rendaw.gitlab.io/blog/2125f09a85f2.html#alsa-exposed)

Les commentaire sont:
**ALSA** = Advanced Linux Sound Architecture. This is the interface provided by the linux kernel to physical sound devices.

**ALSA** = Advanced Linux Sound Architecture. This is a relatively simple userspace API that allows to interact with physical and virtual sound devices. Linux-specific, built directly on top of the other alsa.

ALSA refers variably to either (or sometimes both!) of the above; TFA discusses the userspace portion of ALSA.

**OSS** = Open Sound System. This is a userspace API to access sound devices based special files (/dev/dsp and similar), and was for a long time the primary audio interface on unices and unix-like systems. Older versions of the linux kernel used this, before switching to alsa due to some limitations of the api. FreeBSD, however, still uses oss, having extended the api to eliminate the limitations.

**Pulseaudio**. This is a userspace daemon that provides an interface to sound devices, and is built on top of the kernel-level alsa (but not the user-level alsa). Has some neat networking features. Its design was modelled after that apple’s coreaudio. It primarily favours linux but has ports to most other operating systems.

**JACK** = JACK Audio Connection Kit. Like pulse, this is a userspace daemon that offers sound capabilities. Like pulse, it is quite portable; it can actually run on top of pulse! Its primary aim is to provide reliable latency guarantees to pro audio applications (like DAWs).

**Pipewire**. Yet another daemon. This one aims to be a generic multimedia server—not just for sound—to provide latency guarantees similarly to jack, and to patch up some issues in pulse. It also provides compatibility with jack and pulse; and is somewhat portable, though afaik not so much as pulse.
