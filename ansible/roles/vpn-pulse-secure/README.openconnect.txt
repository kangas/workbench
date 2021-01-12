## README.openconnect.txt

Alternatives to Pulse Secure: OpenConnect
-----------------------------------------

http://www.infradead.org/openconnect/index.html

> OpenConnect is an SSL VPN client initially created to support Cisco's AnyConnect SSL VPN. It has since been ported to support the Juniper SSL VPN (which is now known as Pulse Connect Secure), and the Palo Alto Networks GlobalProtect SSL VPN.

https://wiki.archlinux.org/index.php/OpenConnect

Official source repo:
https://gitlab.com/openconnect/openconnect

	apt show openconnect
	apt show network-manager-openconnect-gnome


Experiment 1, unsuccessful
--------------------------

A quick experiment on Ubuntu 18.04 that did not succeed, but got tantalizingly close:

	sudo apt install network-manager-openconnect-gnome

	$ apt show openconnect
	Package: openconnect
	Version: 7.08-3ubuntu0.18.04.2
	Priority: optional
	Section: universe/net
	Origin: Ubuntu
	Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
	Original-Maintainer: Mike Miller <mtmiller@debian.org>
	Bugs: https://bugs.launchpad.net/ubuntu/+filebug

	$ apt show network-manager-openconnect-gnome
	Package: network-manager-openconnect-gnome
	Version: 1.2.4-1ubuntu1
	Priority: optional
	Section: universe/net
	Source: network-manager-openconnect
	Origin: Ubuntu
	Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
	Original-Maintainer: Mike Miller <mtmiller@debian.org>
	Bugs: https://bugs.launchpad.net/ubuntu/+filebug


Open GNOME Network settings

* VPN: "+"
  * Name: Macmillan Secure
  * VPN Protocol: Juniper/Pulse Network Connect
  * Gateway: vpn.macmillansecure.com/dana-na/auth/url_6/welcome.cgi
  * leave all other fields blank
  * "Apply"
* Switch "On" Macmillan Secure VPN
  * Should show "frmLogin" with two fields, "username:", "password:"

Result was not successful. Log shows:

	GET https://vpn.macmillansecure.com/dana-na/auth/url_6/welcome.cgi
	Attempting to connect to server 199.168.15.23:443
	Connected to 199.168.15.23:443
	SSL negotiation with vpn.macmillansecure.com
	Connected to HTTPS on vpn.macmillansecure.com
	Got HTTP response: HTTP/1.1 200 OK
	Content-Type: text/html; charset=utf-8
	Date: Tue, 12 Jan 2021 01:45:26 GMT
	x-frame-options: SAMEORIGIN
	Connection: close
	Pragma: no-cache
	Cache-Control: no-store
	Expires: -1
	X-XSS-Protection: 1
	Strict-Transport-Security: max-age=31536000
	HTTP body http 1.0 (-1)
	SSL socket closed uncleanly
	Ignoring unknown form submit item 'help'
	POST https://vpn.macmillansecure.com/dana-na/auth/url_6/login.cgi
	SSL negotiation with vpn.macmillansecure.com
	Connected to HTTPS on vpn.macmillansecure.com
	Got HTTP response: HTTP/1.1 302 Moved
	Date: Tue, 12 Jan 2021 01:45:57 GMT
	location: /dana-na/auth/url_6/welcome.cgi?p=no%2Droles
	Content-Type: text/html; charset=utf-8
	Connection: close
	Pragma: no-cache
	Cache-Control: no-store
	Expires: -1
	X-XSS-Protection: 1
	Strict-Transport-Security: max-age=31536000
	HTTP body http 1.0 (-1)
	SSL socket closed uncleanly
	GET https://vpn.macmillansecure.com/dana-na/auth/url_6/welcome.cgi?p=no%2Droles
	SSL negotiation with vpn.macmillansecure.com
	Connected to HTTPS on vpn.macmillansecure.com
	Got HTTP response: HTTP/1.1 200 OK
	Content-Type: text/html; charset=utf-8
	Date: Tue, 12 Jan 2021 01:45:58 GMT
	x-frame-options: SAMEORIGIN
	Connection: close
	Pragma: no-cache
	Cache-Control: no-store
	Expires: -1
	X-XSS-Protection: 1
	Strict-Transport-Security: max-age=31536000
	HTTP body http 1.0 (-1)
	SSL socket closed uncleanly
	Ignoring unknown form submit item 'help'


Experiment 2, unsuccessful
--------------------------

Try using the wrapper script
https://github.com/russdill/juniper-vpn-py

	# provides curl-config, needed below
	sudo apt install libcurl4-openssl-dev

	git clone git@github.com:russdill/juniper-vpn-py.git
	cd juniper-vpn-py
	pip2 install -r requirements.txt

	$ ./juniper-vpn.py --host vpn.macmillansecure.com --username FIRST.LAST --stdin DSID=%DSID% openconnect --juniper %HOST% --cookie-on-stdin
	Username: FIRST.LAST
	Password: 
	Traceback (most recent call last):
	  File "./juniper-vpn.py", line 362, in <module>
	    jvpn.run()
	  File "./juniper-vpn.py", line 166, in run
	    self.action_login()
	  File "./juniper-vpn.py", line 216, in action_login
	    self.br.form['username'] = self.args.username
	  File "/home/kangas/.local/lib/python2.7/site-packages/mechanize/_form_controls.py", line 1945, in __setitem__
	    control = self.find_control(name)
	  File "/home/kangas/.local/lib/python2.7/site-packages/mechanize/_form_controls.py", line 2331, in find_control
	    return self._find_control(name, type, kind, id, label, predicate, nr)
	  File "/home/kangas/.local/lib/python2.7/site-packages/mechanize/_form_controls.py", line 2424, in _find_control
	    raise ControlNotFoundError("no control matching " + description)
	mechanize._form_controls.ControlNotFoundError: no control matching name 'username'
	Terminated
