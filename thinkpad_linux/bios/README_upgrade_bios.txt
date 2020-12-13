2018-03-14

http://www.thinkwiki.org/wiki/BIOS_Upgrade#Using_UEFI
https://troglobit.github.io/2016/07/03/upgrade-x1-carbon-bios-from-linux/

1. Find serial number to identify correct BIOS image

	sudo dmidecode -t system

Find "Product Name" and "Serial Number". Concatenate them with a "-".
Mine is: 20HRS14W00-PF0ZZN98

The current product support page is:
https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-x-series-laptops/thinkpad-x1-carbon-type-20hr-20hq/20hr/s14w00pf0zzn98

Latest available BIOS version:
1.30 released 27 Feb 2018



