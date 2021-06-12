---
layout: default
title: Home
nav_order: 1
description: "ZanzoCam transforms a Raspberry Pi into a low-frequency, autonomous camera."
permalink: /
---

# ZanzoCam
{: .fs-9 }

Low-power, low-frequency, autonomous remote camera based on Raspberry Pi.

{: .fs-6 .fw-300 }

[Download image](https://github.com/zanzocam/zanzocam-core/releases/latest/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [Go to GitHub](https://github.com/zanzocam){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

-----

## Setup

To build a ZanzoCam you need three parts:

 - The [hardware](hardware-setup): a Raspberry Pi Zero W and a few other components, depending on your usecase.
 - The [software](software-setup#raspberry): an OS image to flash on your microSD (you can also [build the system from scratch](image-creation)).
 - A [server](software-setup#server) to receive your pictures: either a simple FTP folder, or a minimal VPC able to run PHP 5+ scripts.

Check out the respective pages for a more detailed look at each of them.

## Features

ZanzoCam is designed for remote, long-term and low-frequency monitoring of remote, off-grid and hard-to-reach locations, so the project focuses on reliability, autonomy and remote configurability.

ZanzoCam supports several hardware configurations to adapt to different usecases, which you can see [here](hardware-setup). They range from a simple WiFi+power line setup to more complex, solar-powered and 3G enabled configurations.

Some relevant features include:

- Can add text or small icons onto the picture, and this text/icons can be changed remotely at any time.
- Works with different Internet access types: WiFi, Ethernet (**soon**), 3G (**soon**).
- Any board compatible with Raspberry Pi OS and `picamera` (image quality is not guaranteed in this case and other bugs might occurr).
- Configurable intervals for shooting pictures, including night break to avoid black images.
- Server hot-swap: no manual intervention required on the device.

### ZanzoCam does *not* need:

- Public and/or static IP address for your Raspberry Pi (the server does need one).
- VPN: you might add it if you wish, but it's not used by the system itself.
- A powerful server (free VPCs are great for this purpose, as long as they can run PHP5+ scripts).

### ZanzoCam does *not* support:

- Video streams
- Direct SSH access (do it yourself with a VPN or a public IP if you wish)
- Any upload protocol other than FTP and HTTP (no Dropbox, no Google Drive, etc)


## Acknowledgements

ZanzoCam has been developed in collaboration with CAI Lombardia, that deployed a several devices running this software on their affiliate huts.
