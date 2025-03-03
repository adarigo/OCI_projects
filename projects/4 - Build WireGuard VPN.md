I wanted to build a VPN server for myself, due to its real practical everyday use and that the free tier is OCI makes this extremely doable.  
I did some research into the types of VPNs I could go with, and I decided on using WireGuard because it was said to be the simplest one to setup as a private VPN.

First step is to load up an instance.  
As usual, default settings with Ubuntu are sufficient and save the private key.  
I named my instance Host VPN for fun.
SSH into the instance with your device and run the update and upgrade commands.  
Also include this one:
~$ sudo apt install wireguard  
This gets WireGuard into our system.  
The setup from here gets a little tricky.  

With WireGuard (wg) now on our system, we need to configure several things.  
1. Create wg0.conf file

