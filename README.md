# OCI_projects

This is the beginning of my cloud journey and I will document my findings. I chose to start with OCI due to the fantastic free tier that is has.

Project 1 - Web server test and VPN

I attempted to start hard and jump directly into building a VPN. I liked the idea of creating a free one for myself so I decided to go for it. Along the way, I loaded up a web server for testing connectivity as you will see below. Good thing I did because I learned a lot from that.

There are a couple of open source VPNs that I was looking through to pick from and after some time, I decided to go with WireGuard due to its perceived simplicity. 

First step was to learn how to navigate the OCI console. After stumbling a bit, I eventually found how to create and activate my very first instance.

I then discovered the importance of SSH that it was necessary to use to be able to access the command line of the instance. I took its public key and loaded it up on an SSH app that I found for my tablet. Made it in!

After I got inside, I had a sense that it was a good idea to do a simple connectivity test before attempting WireGuard. I decided on a simple web server to see if my phone could even access the webpage for it.

I learned how to update and install packages and did so for NGINX, the web server I decided to use.

After some time, I managed to get it running. By entering the IP address over http on my phone, I should be able to connect to the server. However, it wouldn't work. {systemctl} is a command I learned to use frequently and it did appear to be running.

After some research, I learned that I had to configure the security settings in both OCI and the instance to allow network traffic to run over port 80. 

The instance settings in OCI has a security list that is only default set to have port 22 open for SSH. I gave port 80 for http the same access.

Within the instance itself, I learned that the firewall settings have to allow network traffic also. The command for editing these is {iptables}. Similar to the security list, only port 22 was allowed so I configured port 80 access also.

After doing all these, I had successfully connected to the web server on my browser! Things were looking good.

Next step was to install and configure WireGuard for both the instance and my phone.

Created the wg0.conf file and added parameters for the [Interface] and [Peer], the instance and my phone respectively. I set the private IPs for both within 10.0.0.0/24 so that they matched. I had to generate private and public keys for both and exchanged the public keys so they could create the handshake. I also set commands in the conf file to allow traffic to be forwarded between wg0 and ens3. Lastly, I allowed UDP traffic over port 51820 using {iptables} and in the security list.

Despite my efforts though, I could not get the handshake to occur between the two devices. There was still was something blocking it. 

I listened in on traffic from port 51820 with {tcpdump} to see if anything was coming through, which there was, but it was only one packet. When I turned on the VPN from my phone, the instance was only receiving one. I could see if a handshake was being made with {wg show}, but none was.

I double and triple checked everything within OCI, the instance, and my phone, but everything checked out okay. I did more research and concluded that there may be a network issue with being on my phone over mobile data. SSH and http seemed to work fine, but there could have been an issue with UDP or port 51820. 

This was a challenging attempt but I learned a great amount through this. Next plans are to instead create two instances and get them to connect property and eventually having one act as a VPN. This keeps things simpler and bypasses my phone's mobile data.

