## 2018-03-16

My Thinkpad X1 running Ubuntu 17.10 is exhibiting frustrating "bluetooth lag".

- keyboard lag is tolerable, but
- mouse motion is "jumpy"
- with my thinkpad mouse in BT mode, it's jumpy. with the dedicated dongle, it's super smooth.

What gives?

Reading:
https://wiki.archlinux.org/index.php/mouse_polling_rate
https://utcc.utoronto.ca/~cks/space/blog/linux/USBMousePollingRate
https://github.com/raspberrypi/linux/issues/642

These all hint that changing usbhid.mousepoll may be beneficial.

Also, there is a useful program called "evhz" which shows the actual achieved mouse update rate.
https://github.com/ian-kelling/evhz

Initial testing with evhz shows:

```
ThinkPad Compact Bluetooth Keyboard with TrackPoint: Latest  1000Hz, Average   486Hz
ThinkPad Compact Bluetooth Keyboard with TrackPoint: Latest    34Hz, Average   471Hz
ThinkPad Compact Bluetooth Keyboard with TrackPoint: Latest  1000Hz, Average   486Hz
ThinkPad Compact Bluetooth Keyboard with TrackPoint: Latest    34Hz, Average   471Hz
...
(with dedicated dongle)
ThinkPad X1 Mouse: Latest   166Hz, Average   144Hz
ThinkPad X1 Mouse: Latest   100Hz, Average   142Hz
...
(with bluetooth)
ThinkPad X1 Mouse: Latest    22Hz, Average    21Hz
ThinkPad X1 Mouse: Latest    22Hz, Average    21Hz
```

For the keybaord, why the oscillating pattern? Why 34Hz?
For the X1 mouse, why is the Bluetooth version so poor?

Ideally we want a refresh rate of ~125HZ which is double the display refresh rate.

Let's examine the current value of usbhid.mousepoll

```
$ modinfo usbhid | grep ^parm
parm:           mousepoll:Polling interval of mice (uint)
parm:           jspoll:Polling interval of joysticks (uint)
parm:           ignoreled:Autosuspend with active leds (uint)
parm:           quirks:Add/modify USB HID quirks by specifying  quirks=vendorID:productID:quirks where vendorID, productID, and quirks are all in 0x-prefixed hex (array of charp)

$ tail -vn +1 /sys/module/usbhid/parameters/*
==> /sys/module/usbhid/parameters/ignoreled <==
0

==> /sys/module/usbhid/parameters/jspoll <==
0

==> /sys/module/usbhid/parameters/mousepoll <==
0

==> /sys/module/usbhid/parameters/quirks <==
(null),(null),(null),(null)
```

Let's request a 125Hz polling rate:

```
cat "options usbhid mousepoll=8" > /etc/modprobe.d/usbhid.conf

modprobe -r usbhid && modprobe usbhid

$ tail -vn +1 /sys/module/usbhid/parameters/mousepoll 
==> /sys/module/usbhid/parameters/mousepoll <==
8
```

The change took effect. Does it help? NOPE. evhz reports the same results.
So I reverted the change.


------------------------

## 2018-0325

As a test, this morning I rebooted to Windows 10 and paired the X1 mouse via BT.
It worked flawlessly. So the hardware is ok... it seems.

But it seems possible that the issues I'm seeing are specific to the Thinkpad X1 Mouse.

https://bugs.freedesktop.org/show_bug.cgi?id=97813

> Don't be sorry, it's sad Lenovo did not provide enough information (or better drivers) to make this work correctly under linux.
> You spent a lot of time on this issue, including helping and teaching me… thanks

