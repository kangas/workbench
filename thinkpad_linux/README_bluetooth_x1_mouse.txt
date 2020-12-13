Info about Bluetooth mode on the X1 mouse

Why is the X1 mouse unusable under Bluetooth, but ok with the USB dongle?

Best reference:

https://bugs.freedesktop.org/show_bug.cgi?id=97813

 Bug 97813 - slow mouse movement on bluetooth mouse (but not when connected over USB) 

------



Description CircleCode 2016-09-15 07:52:41 UTC

I own a ThinkPad X1 Wireless Touch Mouse (http://shop.lenovo.com/us/en/itemdetails/4X30K40903/460/0E80436C80A748E6AA76791FC42C9CA3),
  connected through bluetooth

the mouse movement on screen is really slow compared to the physical distance.

....

Comment 18 CircleCode 2017-01-13 07:56:21 UTC

Don't be sorry, it's sad Lenovo did not provide enough information (or better drivers) to make this work correctly under linux.  You spent a lot of time on this issue, including helping and teaching me… thanks

Comment 19 Fabrice Bellet 2018-08-27 10:43:16 UTC

The 70ms delay is caused by the bluetooth low energy mode of this mouse. And it can be lowered by reducing the min and max LE connection parameters with "hcitool lecup".

Comment 20 CircleCode 2018-08-28 06:43:06 UTC

As I am not at al a bluetooth expert, I wonder if these parameters can have other side effects (if I understand it correctly, they are applied system-wide)

And as a side question, it seems hcitool is deprecated, so is there another way to achieve the same result?

Last question, for my understanding: what exactly do these parameters?

Comment 21 Fabrice Bellet 2018-09-03 13:26:03 UTC

I'm not a bluetooth expert too, I simply noticed that these parameters solved a similar issue on a completely unrelated bluetooth device, that only shared the characteristic to be a "low energy" device too:

https://github.com/IanHarvey/bluepy/issues/117

My understanding of these min/max values is that they define a fixed rate, used by low energy bluetooth to exchange data, allowing the hardware to enter power saving mode between communications, instead of being always on.

I think these params are specific to a connection, via the handle value.

Comment 22 Peter Hutterer 2018-09-04 04:19:09 UTC

there are several options here for libinput to handle this when detected:
- issue a warning in the log to point to documentation on how to resolve this using the hcitool (or writing to the sysfs files). This is the easiest one to add
- change the connection details itself by writing to the sysfs files. This could be an issue because we may not have write access
- change the connection details by monitoring/writing to the bluez DBus interface. This is the most complex issue because it'd require dbus handling inside libinput which we don't have yet

I think even the first one (log + docs) would be very useful and I'd really appreciate a patch to that extent. Unfortunately I don't have a device to reproduce this here, so I'll rely on you to test the exact sequences of steps.

If you're interested, please open a new issue here: https://gitlab.freedesktop.org/libinput/libinput/issues


