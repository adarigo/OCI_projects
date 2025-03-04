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
1. Create and configure the wg0.conf file.  
2. Install and configure WireGuard on our personal device.  
3. Allow UDP traffic over port 51820.  
4. Enable ip forwarding in the kernel.  

Enter this command to create the configuration file.  
~ $ sudo nano /etc/wireguard/wg0.conf  
Here we need to set a few things in the file.  
Copy and paste the following:  

[Interface]
PrivateKey =                         Address = 192.168.10.1/24  
ListenPort = 51820  
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE  
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o ens3 -j MASQUERADE  

[Peer]
PublicKey = 
AllowedIPs = 192.168.10.2/32

The interface requires a private key from the instance which can be generated.
~ $ wg genkey | tee privatekey | wg pubkey > publickey  

This creates two files, privatekey and publickey.  
The address must be a unique private IP CIDR block that only the VPN will use. Must be different than the VCN CIDR block.  
51820 is the standard port wg operates on.  
PostUp allows the forwarding of packets from any connected peers and changes the IP address of each packet to the instances public IP.  
PostDown removes this should wg close.  
IP forwarding also has to be enabled on the kernel level or this wont work.  
~ $ sudo sysctl -w net.ipv4.ip_forward=1  

Under peer, the public key has to be made from the other device and transferrd over.  
AllowedIP is the specific private IP that selected when configuring wg on the other device.  

Last step on the instance is to allow UDP traffic to flow into the instance.  
In OCI security lists, create an ingress rule to allow UDP over port 51820 to all IP address.  
In the instance, run this command:  
~ $ sudo iptables -A INPUT -p udp --dport 51820 -j ACCEPT  

I used my phone as the peer so the setup here is simpler.  
Download the wg app and create a new tunnel.  
Create a private/public key combo and exchange public keys with the instance.  
Set a private IP address as the same one in the AllowedIP of the peer section of the instance.  
Provide a DNS server, any works.  
Make the endpoint the public ip address of the instance with port 51820.  
Lastly set AllowIPs to 0.0.0.0/0  
This will allow all web traffic to be sent back to your device through the tunnel.  

Everything is ready and its time to load up wg.  
Enter this command:  
~ $ sudo systemctl start wg-quick@wg0  
WireGuard is now running.  
Confirm with this:  
~ $ sudo wg show  
Activate wg on your device.  
A handshake should occur and you are now connected to the instance through the VPN!  
The show command also confirms that a handshake was made.  