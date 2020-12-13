## 2018-05-07
$ sudo apt show fwupd

Description: Firmware update daemon
 fwupd is a daemon to allow session software to update device firmware.
 You can either use a GUI software manager like GNOME Software to view and
 apply updates, the command-line tool or the system D-Bus interface directly.
 Currently, firmware updates using the UEFI capsule format and for the
 ColorHug are supported. More formats may be supported in the future.
 See <https://github.com/hughsie/fwupd> for details

$ man -k fwupd
fwupdate (1)         - update system firmware
fwupdmgr (1)         - fwupd client tool

$ sudo fwupdate -l
{c35736d2-9e47-4578-93e9-68d5b04ea77e} version -1204679327 can be updated to any version above 0
{74997a6b-1adf-4b12-b994-401f06ea8c72} version 65554 can be updated to any version above 0
{798ffd60-f10e-4ac4-8939-c8beabfe55b4} version 65566 can be updated to any version above 65549

$ sudo fwupdmgr get-devices
20HRS14W00 Device Firmware
  DeviceID:             UEFI-c35736d2-9e47-4578-93e9-68d5b04ea77e-dev0
  Guid:                 c35736d2-9e47-4578-93e9-68d5b04ea77e
  Plugin:               uefi
  Flags:                internal|updatable|require-ac|registered|needs-reboot
  Version:              184.50.3425
  VersionLowest:        0.0.1
  Created:              2018-04-30

20HRS14W00 Device Firmware
  DeviceID:             UEFI-74997a6b-1adf-4b12-b994-401f06ea8c72-dev0
  Guid:                 74997a6b-1adf-4b12-b994-401f06ea8c72
  Plugin:               uefi
  Flags:                internal|updatable|require-ac|registered|needs-reboot
  Version:              0.1.18
  VersionLowest:        0.0.1
  Created:              2018-04-30

20HRS14W00 System Firmware
  DeviceID:             UEFI-798ffd60-f10e-4ac4-8939-c8beabfe55b4-dev0
  Guid:                 798ffd60-f10e-4ac4-8939-c8beabfe55b4
  Plugin:               uefi
  Flags:                internal|updatable|require-ac|registered|needs-reboot
  Version:              0.1.30
  VersionLowest:        0.1.14
  Created:              2018-04-30

Integrated Camera
  DeviceID:             usb:00:08
  Guid:                 1b2771c9-68e4-56dd-8c3e-d807388452f3
  Guid:                 f7646995-edc8-56bc-ae10-ee40eebee8db
  Plugin:               usb
  Flags:                registered
  DeviceVendorId:       USB:0x04CA
  Version:              0.22
  Created:              2018-04-30

USB Receiver
  DeviceID:             usb:00:01
  Guid:                 dbf0b98b-73e3-5a7e-80ff-9c055485940d
  Guid:                 2f3b8a45-5b94-5c32-9e6f-ba82e3109f5f
  Plugin:               usb
  Flags:                registered
  DeviceVendorId:       USB:0x046D
  Version:              41.1
  Created:              2018-04-30

HD Graphics 620 (ThinkPad X1 Carbon 5th Gen)
  DeviceID:             ro__sys_devices_pci0000_00_0000_00_02_0
  Guid:                 3ec3df3a-2290-56e5-9d2f-eda62e9ab50b
  Plugin:               udev
  Flags:                internal|registered
  DeviceVendor:         Intel Corporation
  DeviceVendorId:       PCI:0x8086
  Created:              2018-04-30

ThinkPad X1 Carbon Thunderbolt Controller
  DeviceID:             d5010000-0060-5f18-2347-4102d2a17a1e
  Guid:                 89d9d1e6-9e4c-5f07-b5c1-603da3d61835
  Plugin:               thunderbolt
  Flags:                internal|updatable|registered
  DeviceVendor:         Lenovo
  DeviceVendorId:       TBT:0x0109
  Version:              15.0
  Created:              2018-05-07

ThinkPad Thunderbolt 3 Dock
  DeviceID:             00b26980-3e6e-0801-ffff-ffffffffffff
  Guid:                 df675f10-53ac-59ca-bddd-0a86ee492920
  Plugin:               thunderbolt
  Flags:                updatable|registered
  DeviceVendor:         Lenovo
  DeviceVendorId:       TBT:0x0108
  Version:              15.0
  Created:              2018-05-07
