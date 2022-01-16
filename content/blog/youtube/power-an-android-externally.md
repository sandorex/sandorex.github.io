---
title: "Powering an Android Without a Battery"
date: 2021-10-08T08:51:00+02:00
tags: ["electronics", "youtube"]
mathjax: true
---

### Prologue

Im probably not the only one with android phones/tablets laying around with crappy battery which is not worth replacing as the battery costs almost as much as the whole phone[^1]

[^1]: The phone costs \\$30 with ok battery used but the new battery costs \\$20 and it died a year later while the original lasted for 4 years but was pretty crappy after the second

I am one of those pople that still listen to the radio[^2], especially at night so im not woken up by some random noise so after my trusty radio *(that was older than i am)* [^3] started giving me trouble i thought i'd just switch to internet radio and dug up an old LG phone with android 4.4 (LG L7 II P710), *terrible phone btw*

[^2]: Sue me
[^3]: Pretty sure it's more than 25 years old

I found RadioDroid app on F-Droid store cause the phone has 'newer' and cleaner rom which doesn't have play store, maybe i could get it installed but it's slow already with it's 768MB of ram

So after about 2 years of using it as a radio by plugging in speakers and just being plugged into the charger constantly i noticed the screen flexing whenever touched and that got to open it up and **oh boy was the battery swollen**\
That made me look for solutions to power it but i was lazy, so i put it off for a while until...

### Part One and The Finale

This whole post i was gonna cheeze it and i already wrote almost the whole post.. but i never published it and it was really overengineered

I dropped voltage using a powerful diode and then added bunch of 6V3 2200uF caps to smooth that, but the phone does not really like the ripple\
Just go directly with the 5V, should also add a capacitor cause the phone can draw large currents in short bursts and that may turn it off if the wire is thin and long

I am not going to show you how to do it, it's really simple, for the contacts if you want to be able to remove it afterwards just slide male header pins into the contacts for the battery on the phone and hot glue all over it

> **WARNING** Connecting usb while powering it like this probably makes your usb charge your charger or the other way around as the voltages won't match, will this damage something? I do not know, probably not **but you're doing it at your own risk**

### Addendum

If you do get your hands on a phone that can run android recent enough for Termux, with the help of author i made mympd work on it, it has been added as an [official package now](https://github.com/termux/termux-packages/pull/8244)\
And with that *(and bit of pain)* you can setup nice mpd internet radio for your whole house using phone that would sit still and not do anything otherwise
