---
layout: post
title:  "Syncthing - First impression"
date:   2017-07-23 13:46:33 +0100
categories: syncthing emacs org-mode
---

After a recent return to Emacs and [Org Mode][org-mode] I've been looking for a
simple and secure way of synchronising my .org files across different
devices. Mainly my Android phone, Macbook Pro (work), and Dell running Ubuntu
LTS (private). I already have an automated hourly commit of changes to a Git
repository, but I don't find Git convenient when frequently moving between
devices and especially cumbersome on mobile devices.

On Reddit someone mentioned that they were using [Syncthing] and I thought I'd
give it a try. What intrigued me was the element of not having to give your data
to a third-party. Syncthing uses a P2P protocol which means that two linked
devices need to be online at the same time to get synchronised. The main
difference to services like [Dropbox] and [Google Drive][google-drive] is that
there is no centralised server. 

I installed the client on all of the devices (described below) and got
started. The first hurdle was to figure out how to connect two
devices. Fortunately the client on my Macbook was able to produce a QR code that
the client on the phone would recognise and easily added the device to the
phone. When I added the Macbook/org folder to the phone I had to wait a bit
before the Macbook asked if I would approve the connection to the phone. All
good. I was able to access my org-mode file on my phone using [Orgzly].

I assume that the Syncthing client will drain the phone battery and only start
the client when I want to update my notes. This is not ideal, but gives me more
control than I have over the numerous other backgrounds jobs running on my
phone.

Now that my Macbook and phone successfully synchronise .org files I want to
expand the setup a bit. Given that the phone is only online when I use it and
that my Macbook frequently is in sleep mode there is a need for a device that is
always online. Being a bit short on time I decided to attempt using my old phone
with a broken screen as the always on device. I installed the app and connected
the phone with the two other devices. Initially this setup worked
fine. Syncthing was able to connect to the phone from my Macbook at work, but in
the long run it seems that issues with power saving takes the phone offline and I
found, not surprisingly, the phone solution to be quite unstable.

Having a Raspberry PI without a purpose in the house I thought it would be a good
replacement for the flaky phone solution. Installing Syncthing on the PI was as
easy as on Ubuntu. To access the web interface on the PI I used SSH tunnelling
from my Macbook:

```
$ ssh -L 8888:localhost:8384 <raspianpi IP> 
```

I was then able to access the web interface on my Macbook at
http://localhost:8888. Next I added my Macbook, phone and Dell to synchronise
with the Raspberry PI. The synchronisation now works perfectly. I am considering
buying an external disk for the Raspberry PI and use it to store media
files. First I want to experiment a bit more with the org folder I have set up.

I recomment trying [Syncthing] if you are looking for an alternative to the
established file-sync services and if you like it support the team with a
donation to keep their infratructure running; I know I will.




## Installation

It was very easy to get Syncthing install across my devices. I
followed the instructions below.

### Mac

```bash 
$ brew install syncthing 
``` 
To automatically start Syncthing I also added
```bash
$ brew services start syncthing
```


### Android 

The Android app is available in Google Play - [Syncthing App][syncthing-app]

and Orgzly can be found here - [Orgzly]


### Ubuntu and Raspbian

For my Ubuntu desktop I followed the instructions from [apt.syncthing.net]:

```bash 
# Add the release PGP keys: 
$ curl -s https://syncthing.net/release-key.txt | sudo apt-key add -

# Add the "stable" channel to your APT sources: 
$ echo "deb https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list

# Update and install syncthing: 
$ sudo apt-get update sudo apt-get install syncthing

```


[Syncthing]: https://syncthing.net/ 
[syncthing-app]:https://play.google.com/store/apps/details?id=com.nutomic.syncthingandroid&hl=en
[apt.syncthing.net]: https://apt.syncthing.net/ 
[Dropbox]:https://www.dropbox.com/ 
[Orgzly]: https://play.google.com/store/apps/details?id=com.orgzly
[google-drive]: https://drive.google.com
[org-mode]: http://orgmode.org/
