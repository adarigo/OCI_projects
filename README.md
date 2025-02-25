# OCI_projects

This is the beginning of my cloud journey and I will document my findings. I chose to start with OCI due to the fantastic free tier that is has.

Project 1 - Web server test and VPN

I attempted to start hard and jump directly into building a VPN. I liked the idea of creating a free one for myself so I decided to go for it. Along the way, I loaded up a web server for testing connectivity as you will see below. Good thing I did because I learned a lot from that.

There are a couple of open source VPNs that I was looking through to pick from and after some time, I decided to go with WireGuard due to its perceived simplicity. 

First step was to learn how to navigate the OCI console. After stumbling a bit, I eventually found how to create and activate my very first instance.

I then discovered the importance of SSH that it was necessary to use to be able to access the command line of the instance. I took its public key and loaded it up on an SSH app that I found for my tablet. Made it in!

After I got inside, I had a sense that it was a good idea to do a simple connectivity test before attempting WireGuard. I decided on a simple web server to see if my phone could even access the webpage for it.

I learned how to update and install packages and did so for NGINX, the web server I decided to use.

After some time, I managed to get it running. By entering the IP address over http on my phone, I should be able to connect to the server. However, it wouldn't work. (systemctl) is a command I learned to use frequently and it did appear to be running.

After some research, I learned that I had to configure the security settings in both OCI and the instance to allow network traffic to run over port 80. 

The instance settings in OCI has a security list that is only default set to have port 22 open for SSH. I gave port 80 for http the same access.

Within the instance itself, I learned that the firewall settings have to allow network traffic also. The command for editing these is (iptables). Similar to the security list, only port 22 was allowed so I configured port 80 access also.

After doing all these, I had successfully connected to the web server on my browser! Things were looking good.

Next step was to install and configure WireGuard for both the instance and my phone.


