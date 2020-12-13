## 2018-03-15

```
$  uname -a
Linux qwghlm 4.13.0-37-generic #42-Ubuntu SMP Wed Mar 7 14:13:23 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux

$ lspci -nnk | grep -iA2 net
00:1f.6 Ethernet controller [0200]: Intel Corporation Ethernet Connection (4) I219-V [8086:15d8] (rev 21)
	Subsystem: Lenovo Ethernet Connection (4) I219-V (ThinkPad X1 Carbon 5th Gen) [17aa:224f]
	Kernel driver in use: e1000e
	Kernel modules: e1000e
--
04:00.0 Network controller [0280]: Intel Corporation Wireless 8265 / 8275 [8086:24fd] (rev 88)
	Subsystem: Intel Corporation Wireless 8265 / 8275 (Dual Band Wireless-AC 8265) [8086:1130]
	Kernel driver in use: iwlwifi

$ lsusb
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 005: ID 138a:0097 Validity Sensors, Inc. 
Bus 001 Device 004: ID 04ca:7067 Lite-On Technology Corp. 
Bus 001 Device 003: ID 8087:0a2b Intel Corp. 
Bus 001 Device 002: ID 046d:c534 Logitech, Inc. Unifying Receiver
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

$ modinfo iwlwifi | grep ^parm
parm:           swcrypto:using crypto in software (default 0 [hardware]) (int)
parm:           11n_disable:disable 11n functionality, bitmap: 1: full, 2: disable agg TX, 4: disable agg RX, 8 enable agg TX (uint)
parm:           amsdu_size:amsdu size 0: 12K for multi Rx queue devices, 4K for other devices 1:4K 2:8K 3:12K (default 0) (int)
parm:           fw_restart:restart firmware in case of error (default true) (bool)
parm:           antenna_coupling:specify antenna coupling in dB (default: 0 dB) (int)
parm:           nvm_file:NVM file name (charp)
parm:           d0i3_disable:disable d0i3 functionality (default: Y) (bool)
parm:           lar_disable:disable LAR functionality (default: N) (bool)
parm:           uapsd_disable:disable U-APSD functionality bitmap 1: BSS 2: P2P Client (default: 3) (uint)
parm:           bt_coex_active:enable wifi/bt co-exist (default: enable) (bool)
parm:           led_mode:0=system default, 1=On(RF On)/Off(RF Off), 2=blinking, 3=Off (default: 0) (int)
parm:           power_save:enable WiFi power management (default: disable) (bool)
parm:           power_level:default power save level (range from 1 - 5, default: 1) (int)
parm:           fw_monitor:firmware monitor - to debug FW (default: false - needs lots of memory) (bool)
parm:           d0i3_timeout:Timeout to D0i3 entry when idle (ms) (uint)
parm:           disable_11ac:Disable VHT capabilities (default: false) (bool)

$ grep "" /sys/module/iwlwifi/parameters/*
/sys/module/iwlwifi/parameters/11n_disable:0
/sys/module/iwlwifi/parameters/amsdu_size:0
/sys/module/iwlwifi/parameters/antenna_coupling:0
/sys/module/iwlwifi/parameters/bt_coex_active:Y
/sys/module/iwlwifi/parameters/d0i3_disable:Y
/sys/module/iwlwifi/parameters/d0i3_timeout:1000
/sys/module/iwlwifi/parameters/disable_11ac:N
/sys/module/iwlwifi/parameters/fw_monitor:N
/sys/module/iwlwifi/parameters/fw_restart:Y
/sys/module/iwlwifi/parameters/lar_disable:N
/sys/module/iwlwifi/parameters/led_mode:0
/sys/module/iwlwifi/parameters/nvm_file:(null)
/sys/module/iwlwifi/parameters/power_level:0
/sys/module/iwlwifi/parameters/power_save:N
/sys/module/iwlwifi/parameters/swcrypto:0
/sys/module/iwlwifi/parameters/uapsd_disable:3
```

Reading:
https://askubuntu.com/questions/663500/ubuntu-14-04-bluetooth-mouse-lags-time-by-time

Suggests:

> Run in terminal
> 
> $ sudo cp /etc/modprobe.d/iwlwifi.conf /etc/modprobe.d/iwlwifi.conf.backup
> 
> $ sudo echo "options iwlwifi bt_coex_active=0" >> /etc/modprobe.d/iwlwifi.conf
> 
> and reboot.
> 
> Bluetooth coexistence technology in iwlwifi is not good and makes things worse.


DONE:
```
cp /etc/modprobe.d/iwlwifi.conf /etc/modprobe.d/iwlwifi.conf.ORIG
echo "options iwlwifi bt_coex_active=0" >> /etc/modprobe.d/iwlwifi.conf
```

Rebooted. SAW NO DIFFERENCE. REVERTED.

Reference:
https://wiki.debian.org/iwlwifi

-----------------

## 2018-04-30 - iwlwifi firmware crash

https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi/debugging

$ ethtool -i wlp4s0
	driver: iwlwifi
	version: 4.15.17-041517-generic
	firmware-version: 31.560484.0
	expansion-rom-version: 
	bus-info: 0000:04:00.0
	supports-statistics: yes
	supports-test: no
	supports-eeprom-access: no
	supports-register-dump: no
	supports-priv-flags: no

$ dmesg

	[146696.774122] iwlwifi 0000:04:00.0: Queue 5 is active on fifo 3 and stuck for 10000 ms. SW [132, 198] HW [132, 198] FH TRB=0x080305093
	[146696.774132] iwlwifi 0000:04:00.0: Queue 10 is active on fifo 1 and stuck for 10000 ms. SW [41, 90] HW [41, 90] FH TRB=0x0c010a038
	[146696.774419] iwlwifi 0000:04:00.0: Microcode SW error detected.  Restarting 0x2000000.
	[146696.774638] iwlwifi 0000:04:00.0: Start IWL Error Log Dump:
	[146696.774647] iwlwifi 0000:04:00.0: Status: 0x00000100, count: 6
	[146696.774655] iwlwifi 0000:04:00.0: Loaded firmware version: 31.560484.0
	[146696.774664] iwlwifi 0000:04:00.0: 0x00000084 | NMI_INTERRUPT_UNKNOWN       
	[146696.774672] iwlwifi 0000:04:00.0: 0x000002F0 | trm_hw_status0
	[146696.774679] iwlwifi 0000:04:00.0: 0x00000000 | trm_hw_status1
	[146696.774686] iwlwifi 0000:04:00.0: 0x0002495C | branchlink2
	[146696.774693] iwlwifi 0000:04:00.0: 0x0003962E | interruptlink1
	[146696.774700] iwlwifi 0000:04:00.0: 0x0003962E | interruptlink2
	[146696.774706] iwlwifi 0000:04:00.0: 0x00000000 | data1
	[146696.774713] iwlwifi 0000:04:00.0: 0x00000080 | data2
	[146696.774720] iwlwifi 0000:04:00.0: 0x07830000 | data3
	[146696.774727] iwlwifi 0000:04:00.0: 0xA98060AB | beacon time
	[146696.774734] iwlwifi 0000:04:00.0: 0x37A5C7B9 | tsf low
	[146696.774741] iwlwifi 0000:04:00.0: 0x000007F2 | tsf hi
	[146696.774748] iwlwifi 0000:04:00.0: 0x00000000 | time gp1
	[146696.774755] iwlwifi 0000:04:00.0: 0x248C1F6B | time gp2
	[146696.774762] iwlwifi 0000:04:00.0: 0x00000001 | uCode revision type
	[146696.774769] iwlwifi 0000:04:00.0: 0x0000001F | uCode version major
	[146696.774776] iwlwifi 0000:04:00.0: 0x00088D64 | uCode version minor
	[146696.774783] iwlwifi 0000:04:00.0: 0x00000230 | hw version
	[146696.774790] iwlwifi 0000:04:00.0: 0x00C89000 | board version
	[146696.774797] iwlwifi 0000:04:00.0: 0x006F0400 | hcmd
	[146696.774803] iwlwifi 0000:04:00.0: 0x0002200A | isr0
	[146696.774810] iwlwifi 0000:04:00.0: 0x00800000 | isr1
	[146696.774817] iwlwifi 0000:04:00.0: 0x0800180A | isr2
	[146696.774824] iwlwifi 0000:04:00.0: 0x004100C0 | isr3
	[146696.774831] iwlwifi 0000:04:00.0: 0x00000000 | isr4
	[146696.774838] iwlwifi 0000:04:00.0: 0x006F0400 | last cmd Id
	[146696.774844] iwlwifi 0000:04:00.0: 0x00000000 | wait_event
	[146696.774851] iwlwifi 0000:04:00.0: 0x00000000 | l2p_control
	[146696.774858] iwlwifi 0000:04:00.0: 0x00000020 | l2p_duration
	[146696.774865] iwlwifi 0000:04:00.0: 0x00000000 | l2p_mhvalid
	[146696.774872] iwlwifi 0000:04:00.0: 0x00000080 | l2p_addr_match
	[146696.774879] iwlwifi 0000:04:00.0: 0x0000000D | lmpm_pmg_sel
	[146696.774886] iwlwifi 0000:04:00.0: 0x13091828 | timestamp
	[146696.774893] iwlwifi 0000:04:00.0: 0x00340010 | flow_handler
	[146696.775065] iwlwifi 0000:04:00.0: Start IWL Error Log Dump:
	[146696.775073] iwlwifi 0000:04:00.0: Status: 0x00000100, count: 7
	[146696.775081] iwlwifi 0000:04:00.0: 0x00000070 | ADVANCED_SYSASSERT
	[146696.775088] iwlwifi 0000:04:00.0: 0x00000000 | umac branchlink1
	[146696.775095] iwlwifi 0000:04:00.0: 0xC0086950 | umac branchlink2
	[146696.775102] iwlwifi 0000:04:00.0: 0xC00842BC | umac interruptlink1
	[146696.775109] iwlwifi 0000:04:00.0: 0xC00842BC | umac interruptlink2
	[146696.775116] iwlwifi 0000:04:00.0: 0x00000800 | umac data1
	[146696.775122] iwlwifi 0000:04:00.0: 0xC00842BC | umac data2
	[146696.775129] iwlwifi 0000:04:00.0: 0xDEADBEEF | umac data3
	[146696.775136] iwlwifi 0000:04:00.0: 0x0000001F | umac major
	[146696.775143] iwlwifi 0000:04:00.0: 0x00088D64 | umac minor
	[146696.775150] iwlwifi 0000:04:00.0: 0xC088627C | frame pointer
	[146696.775157] iwlwifi 0000:04:00.0: 0xC088627C | stack pointer
	[146696.775163] iwlwifi 0000:04:00.0: 0x006F0400 | last host cmd
	[146696.775170] iwlwifi 0000:04:00.0: 0x00000000 | isr status reg
	[146696.775183] ieee80211 phy0: Hardware restart was requested

----

https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1710390

Claims:

	Ok, seems that there is a workaround for this problem. Updating /etc/NetworkManager/NetworkManager.conf and adding this to the file did solve the problem.

	[device]
	wifi.scan-rand-mac-address=no

But what does this accomplish?

https://blogs.gnome.org/thaller/2016/08/26/mac-address-spoofing-in-networkmanager-1-4-0/

Ah, MAC randomization during network scan.

```
$ nmcli
wlp4s0: connected to HBexec2
	"Intel Wireless 8265 / 8275 (Dual Band Wireless-AC 8265)"
	wifi (iwlwifi), A0:88:69:E1:CE:B8, hw, mtu 1500
	ip4 default
	inet4 10.9.138.107/23
	route4 169.254.0.0/16
	inet6 fe80::e16:d660:c7fa:ce8a/64

DNS configuration:
	servers: 10.9.156.248 10.9.156.249
	domains: newyork.hbpub.net
	interface: wlp4s0

Use "nmcli device show" to get complete information about known devices and
"nmcli connection show" to get an overview on active connection profiles.

Consult nmcli(1) and nmcli-examples(5) manual pages for complete usage details.
```

Tweaks performed today:

	nmcli connection show HBExec2
	nmcli connection show modify HBexec2 wifi.cloned-mac-address permanent
	nmcli connection up HBexec2
	ip link show

----

## 2018-05-09

Found this:
https://bugzilla.kernel.org/show_bug.cgi?id=172431

So what is my wifi card?

https://www.intel.com/content/dam/www/public/us/en/documents/product-briefs/dual-band-wireless-ac-8265-brief.pdf

What is the driver?

https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi

What version of firmware am I loading?

[    8.887335] iwlwifi 0000:04:00.0: enabling device (0000 -> 0002)
[    8.891120] input: ThinkPad Extra Buttons as /devices/platform/thinkpad_acpi/input/input9
[    8.895513] iwlwifi 0000:04:00.0: Direct firmware load for iwlwifi-8265-34.ucode failed with error -2
[    8.895642] iwlwifi 0000:04:00.0: Direct firmware load for iwlwifi-8265-33.ucode failed with error -2
[    8.895654] iwlwifi 0000:04:00.0: Direct firmware load for iwlwifi-8265-32.ucode failed with error -2
[    8.902068] iwlwifi 0000:04:00.0: loaded firmware version 31.560484.0 op_mode iwlmvm

Aha, we could load -34 but we actually load version 31.

$ ls /lib/firmware/iwlwifi-8265-*
/lib/firmware/iwlwifi-8265-21.ucode  /lib/firmware/iwlwifi-8265-27.ucode
/lib/firmware/iwlwifi-8265-22.ucode  /lib/firmware/iwlwifi-8265-31.ucode

The -31 file is provided by the 'linux-firmware' package, naturally.

$ dpkg -S /lib/firmware/iwlwifi-8265-31.ucode 
linux-firmware: /lib/firmware/iwlwifi-8265-31.ucode

$ dpkg -l linux-firmware
||/ Name                   Version          Architecture     Description
+++-======================-================-================-==================================================
ii  linux-firmware         1.169.3          all              Firmware for Linux kernel drivers

1.169.3 is the current correct package for artful
https://launchpad.net/ubuntu/artful/+source/linux-firmware

Are there newer versions? Yes:
1.169.4 is "proposed" for Artful
1.173 is current for Bionic

https://launchpad.net/ubuntu/+source/linux-firmware/1.173
https://launchpad.net/ubuntu/+archive/primary/+sourcefiles/linux-firmware/1.173/linux-firmware_1.173.tar.gz

Does the 1.173 package provide a newer file? YES.

$ tar tzf linux-firmware_1.173.tar.gz | grep iwlwifi-8265
linux-firmware/iwlwifi-8265-21.ucode
linux-firmware/iwlwifi-8265-22.ucode
linux-firmware/iwlwifi-8265-27.ucode
linux-firmware/iwlwifi-8265-31.ucode
linux-firmware/iwlwifi-8265-34.ucode

Sweet. Let's extract that one file and put it into place. I wonder if that will work?

```
$ sha256sum linux-firmware_1.173.tar.gz ; echo '9b5b112093a8f4c0f313e3353ebb4aae9207d8b67e4a50576915b9845e37cf22'
9b5b112093a8f4c0f313e3353ebb4aae9207d8b67e4a50576915b9845e37cf22  linux-firmware_1.173.tar.gz
9b5b112093a8f4c0f313e3353ebb4aae9207d8b67e4a50576915b9845e37cf22

$ tar xzf linux-firmware_1.173.tar.gz linux-firmware/iwlwifi-8265-34.ucode

$ sudo cp linux-firmware/iwlwifi-8265-34.ucode /lib/firmware
[sudo] password for kangas: 

$ ls -l /lib/firmware/iwlwifi-8265-34.ucode
-rw-r--r-- 1 root root 2440780 May  9 18:44 /lib/firmware/iwlwifi-8265-34.ucode

$ dpkg -S /lib/firmware/iwlwifi-8265-34.ucode
dpkg-query: no path found matching pattern /lib/firmware/iwlwifi-8265-34.ucode

$ sudo modprobe -r iwlwifi && sudo modprobe iwlwifi

$ dmesg -wT

[Wed May  9 18:51:46 2018] wlp4s0: deauthenticating from 00:7f:28:5a:7f:9c by local choice (Reason: 3=DEAUTH_LEAVING)
[Wed May  9 18:51:46 2018] wlp4s0: failed to remove key (1, ff:ff:ff:ff:ff:ff) from hardware (-22)
[Wed May  9 18:51:46 2018] wlp4s0: failed to remove key (2, ff:ff:ff:ff:ff:ff) from hardware (-22)
[Wed May  9 18:51:46 2018] cfg80211: Loading compiled-in X.509 certificates for regulatory database
[Wed May  9 18:51:46 2018] cfg80211: Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
[Wed May  9 18:51:46 2018] platform regulatory.0: Direct firmware load for regulatory.db failed with error -2
[Wed May  9 18:51:46 2018] cfg80211: failed to load regulatory.db
[Wed May  9 18:51:46 2018] Intel(R) Wireless WiFi driver for Linux
[Wed May  9 18:51:46 2018] Copyright(c) 2003- 2015 Intel Corporation
[Wed May  9 18:51:46 2018] iwlwifi 0000:04:00.0: loaded firmware version 34.0.1 op_mode iwlmvm
[Wed May  9 18:51:47 2018] iwlwifi 0000:04:00.0: Detected Intel(R) Dual Band Wireless AC 8265, REV=0x230

$ ethtool -i wlp4s0 | grep firmware-version
firmware-version: 34.0.1
```

NICE! Firmware version 34 is loaded.

Fingers crossed that this fixes something... :|
