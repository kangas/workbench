Pulse Secure VPN client setup
-------------------------------

Pulse Secure was formerly Junos Pulse from Juniper Networks.
https://www.pulsesecure.net/

Status: As of October 2020, I have tested this with

- Pulse Version: 9.1R8(165)
- Ubuntu 18.04 and 20.04

The Linux client works well but is annoying to download. It is not available on any public URL. Instead, you must use the following web form to request a download link.

https://www.pulsesecure.net/trynow/client-download/

As of October 2020, this web form does not work on Firefox!! But it does work with Chrome.

Required information:

- First name
- Last name
- Business email
- Country
- State
- Postal Code

After a successful form submission, you should then receive an email with subject "[trial details] Pulse Client". Download links inside include:

- Ubuntu/Debian 64 Bit Installer
- CentOS/RHEL 64 Bit Installer
- MD5

Each download link is of the form:
http://go.pulsesecure.net/WS0e7qNqN0P3Efk00h0b400

Download the "Ubuntu/Debian 64 Bit Installer" and install via "dpkg -i".
By default it installs in /usr/local/pulse/

Once the package is installed, you may find that it fails due to missing shared library dependencies.

	$ /usr/local/pulse/pulseUi
	/usr/local/pulse/pulseUi: error while loading shared libraries: libwebkitgtk-1.0.so.0: cannot open shared object file: No such file or directory

See discussions here:

- https://community.pulsesecure.net/t5/Pulse-Desktop-Clients/pulseUi-doesn-t-work-in-ubuntu-20-04/td-p/42721
- https://askubuntu.com/questions/1135065/cant-run-pulse-secure-on-ubuntu-19-04-because-libwebkitgtk-1-0-so-0-is-missing

This role contains workarouns for the missing libraries for Ubuntu 20.04.

More information about the Linux client can be found here:

	https://kb.pulsesecure.net/articles/Pulse_Secure_Article/KB40126/

	KB40126 - How to use the Pulse Secure Linux CLI client.

	This article describes the steps to install the Pulse client on Linux systems and the commands needed to initiate a VPN session.  Pulse Linux client is available with the release of Pulse Connect Secure 8.1R7 and above.

	Note: As a reminder, a valid login ID on https://my.pulsesecure.net is required to download software from the Pulse Secure Licensing and Download Center. 


Alternatives to Pulse Secure: OpenConnect
-----------------------------------------

See README.openconnect.txt


Alternatives: chroot
--------------------

Consider installing the official Pulse Secure VPN client inside a chroot, so its shared library dependencies can be decoupled from the host OS libraries.

https://wiki.archlinux.org/index.php/Chroot
https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot
https://help.ubuntu.com/community/BasicChroot
https://help.ubuntu.com/community/DebootstrapChroot
