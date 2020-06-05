---
title: "Linux Issues"
date: 2020-06-05T16:27:23+02:00
tags: ["linux"]
url: "/linux-issues"
---

This post will contain all the issues i've experienced on linux, i will update it every time there's a new one or i've found a new fix

### GDM black screen until tty is flipped
Fedora Workstation 31 using Nvidia Proprietary Drivers on Xorg

I have been able to find a fix so i simply switched to `lightdm`

### Random plymouth theme breakage
Fedora Workstation 31 using Nvidia Proprietary Drivers on Xorg  
(Not exclusive to fedora in my experience)

My plymouth theme suddenly changed and broke and after an update, the fix was quite simple i just changed the theme then reverted it to the original one like
```console
# plymouth-set-default-theme -R text
# plymouth-set-default-theme -R charge # my favorite theme
```

The `-R` makes it rebuild initrd which is required after a theme change

### Suspend failing sometimes because of an USB controller
Fedora Workstation 31 using Nvidia Proprietary Drivers on Xorg

The computer suspended properly sometimes, but other times the LED that's supposed to blink is simply on and some fans spin, but there's no screen output

This has frustrated the hell out of me for months until i caught the error in the `journalctl`, somehow i didnt see it before

```console
# journalctl -b -r # get reversed output of current boot
...
Jun 03 11:13:16 aurum kernel: PM: Some devices failed to suspend, or early wake event detected
Jun 03 11:13:16 aurum kernel: PM: Device 0000:00:12.0 failed to suspend async: error -16
Jun 03 11:13:16 aurum kernel: PM: dpm_run_callback(): pci_pm_suspend+0x0/0x160 returns -16
Jun 03 11:13:16 aurum kernel: PM: pci_pm_suspend(): hcd_pci_suspend+0x0/0x30 returns -16
0/0x30 returns -16
...
```

After boggling my head for a while i found out the `0000:00:12.0` is a USB controller
```console
$ lspci -v | grep 00:12.0
00:12.0 USB controller: Advanced Micro Devices, Inc. [AMD/ATI] SB7x0/SB8x0/SB9x0 USB OHCI0 Controller (prog-if 10 [OHCI])
```

So the controller (aka USB hub) didn't want to suspend properly and the suspend fails, the fix was simple enough disable the controller before sleep and enable it after resume

Disable it with
```console
# sh -c "echo '0000:00:12.0' > /sys/bus/pci/drivers/ohci-pci/unbind"
```

Enable it with
```console
# sh -c "echo '0000:00:12.0' > /sys/bus/pci/drivers/ohci-pci/bind"
```

Do note that `ohci-pci` can be different for different controllers

---

The fix (systemd services with install script) can be found [here](https://github.com/sandorex/dotfiles/tree/086159d2fe807296e8911c903f73ca02e1d14350/fixes/usb-controller-preventing-sleep)
