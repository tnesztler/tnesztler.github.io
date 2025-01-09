---
title: Stream from VLC using Google Cast (Chromecast) with pfSense across VLANs
date: 2025-01-08 22:05:00 +01:00
categories: [pfsense]
tags: [chromecast, googlecast, vlc, vlan, avahi, pfsense, rules, firewall]
---

In this post, I'll show how you can setup [pfSense](https://www.pfsense.org) to enable streaming videos from [VLC](https://www.videolan.org/vlc/) to a device such as a TV using [Google Cast](https://en.wikipedia.org/wiki/Google_Cast) (aka. Chromecast) across VLANs.

## Prerequisites

- A pfSense firewall
- The [Avahi](https://docs.netgate.com/pfsense/en/latest/packages/avahi.html) package installed on pfSense

## Configuration

### Avahi

In order to relay the [Multicast DNS](https://en.wikipedia.org/wiki/Multicast_DNS) (mDNS) packets used by Google Cast from one network to another (on different VLANs), you need to use a service such as Avahi on the firewall.

In terms of configuration, you must under `Services > Avahi`:

- Enable the daemon
- Select the interfaces you want communication across
- Check `Disable IPv6` if you don't use it
- Check `Enable reflection`
- In reflection filtering, add the `_googlecast._tcp.local` service to limit mDNS requests to Google Cast

### Firewall Rules

Under `Firewall > Rules`,

1. From the **sending** network (ie. from which you will cast the audio or video), you must allow **TCP/8009** (the [Chromecast control port](https://github.com/videolan/vlc/blob/master/modules/stream_out/chromecast/chromecast.h#L62)) traffic to the **receiving** device (ie. where the video will be cast to such as your TV).
2. From the **receiving** device, you must allow **TCP/8010** traffic (the VLC [HTTP port](https://github.com/videolan/vlc/blob/master/modules/stream_out/chromecast/chromecast.h#L63)) to the **sending** device as VLC acts as web server that can stream video. I personally simplified the rule with a destination being an entire network and not just a device, since I want to stream from my phone or computer easily (both on the same network).

On the **sending** interface (eg. a wireless network):

| Protocol |      Source      | Port | Destination | Port |
| :------: | :--------------: | :--: | :---------: | :--: |
|   IPv4   | WIRELESS subnets |  \*  | TV address  | 8009 |

On the **receiving** interface (eg. an IoT network):

| Protocol |   Source   | Port |   Destination    | Port |
| :------: | :--------: | :--: | :--------------: | :--: |
|   IPv4   | TV address |  \*  | WIRELESS subnets | 8010 |

Note: The TCP/8010 HTTP port is the one used by default by VLC and can be changed in the settings.

## Google Cast

This will also enable you to use Google Cast as well from other sources than VLC such as various apps on your phone or computer.
