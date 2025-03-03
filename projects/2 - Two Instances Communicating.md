I wanted to see if I could manage to get two instances to send messages to each other.
The goal was to use netcat to connect the two and send messages. Here are the steps I took.

Create two instances that use the default settings in OCI with Ubuntu 22.04.  
Save both of the private keys for each instance.  
Use my device's terminal to SSH into both instances.  
Run the following commands to upgrade or install netcat.  
  ~ $ sudo apt update  
  ~ $ sudo apt install netcat-openbsd  

Try to getting netcat running on a random port.  
I used port 9000.  
Instance one run:  
  ~$ nc -lp 9000  
Instance two run:  
  ~$ nc [host-public-ip] 9000  

It doesn't work.  

I discovered about the need to make changes to the security lists VCN and firewall within the instances.  
For the VCN security list, an ingress rule must be applied to allow all IPs to connect to port 9000 over TCP.
In both instances the command below must be added.
  ~$ sudo iptables -A INPUT -p tcp --dport 9000 -j ACCEPT  
The input chain in the firewall now allows TCP over port 9000.  

Check the tables.  
  ~$ sudo iptables -L INPUT  
Make sure there are no reject all or drop all commands above this one otherwise it will fail.  

Try netcat again.  
Host instance run:  
  ~$ nc -lp 9000  
Client instance run:  
  ~$ nc [host-public-ip] 9000  

Both instances should now be waiting and ready to either send or receive messages.  
When sending a message, it will appear on the other instance.  

