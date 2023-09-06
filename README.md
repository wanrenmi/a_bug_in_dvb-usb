# a_bug_in_dvb-usb
A denial of service vulnerability exists in the drivers/media/usb/dvb-usb module in linux kernel 6.4.10. The attack scenario is that an attacker can use our PoC as a USB descriptor to build a virtual or physical USB device, and then plug it into the USB port of a host using the linux kernel. No permissions are required to trigger the vulnerability, but physical contact is required.
The USB kernel module of the victim machine initializes the dvb-usb device according to its content after recognizing the request of the dvb device. The function "dvb_usb_read_remote_control" registers the polling function without checking the content of the "work" structure's member data. The "usb_bulk_message" then is required to send data with length 0 again and again, and the system resources are consumed in an infinite loop.

The poc we provide is as follows:
![image](https://github.com/wanrenmi/a_bug_in_dvb-usb/assets/42407501/f0a06534-2f39-4ec2-aea7-984fa0756d6c)

The vulnerability trigger effect is as follows:
![image](https://github.com/wanrenmi/a_bug_in_dvb-usb/assets/42407501/3ee9b361-6cc2-47aa-b399-5ed67a72cc5a)

This vulnerability is more serious in linux kernel versions prior to 5.6.19 and can directly cause a kernel crash to print out the call trace.
![image](https://github.com/wanrenmi/a_bug_in_dvb-usb/assets/42407501/640b04c9-bcbf-4afd-9d9a-3391e69dbe2f)
