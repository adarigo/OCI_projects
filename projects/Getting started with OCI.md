Learned about what OCI is and how it compares to other cloud providers.  
Created an account and began to explore the main page.  
First step was to create a virtual machine.  
This can be found under instances and select Create Instance.  
Name the instance anything you'd like.  

Placement: Leave alone  
Security: Leave alone  
Image and Shape: Select Ubuntu 22.04 and VM.Standard.E2.1.Micro.  
    Chose Ubuntu for simplicity and the shape is part of OCI's free tier  
Primary VNIC: Can leave as the default ones.  
The option to make your own is there if needed.  
Primary VNIC IP address: Leave alone  
Add SSH keys: Leave alone  
    Downloaded the private SSH key because that will be needed  
Boot Volume: Leave alone  
Block Volume: Leave alone  
Live migration: Leave alone  
Select create and the virual machine is ready to go.  

Next step is to access the instance.  
I downloaded Termux which is a Linux terminal for Android and used that.  
To get into the instance we need a couple things.  
    the instance's public IP address  
    the instance's private key  
The command ssh can be used to enter into the instance with this information.  
    ~ $ ssh ubuntu@<instance public IP> -i /filepath/privatekey.key  
ubuntu is the default username and -i identifies the file holding the private key  
We are in.  

There are a varity of commands that can be used to test or check things.  
I'll list a few  
    ~ $ cd <folder>  
    ~ $ ls  
    ~ $ pwd  
    ~ $ ip a  
    ~ $ whoami  
    ~ $ uname -a  
    ~ $ ping 8.8.8.8  
    ~ $ curl ifconfig.me  
    ~ $ sudo apt update  
    ~ $ sudo apt upgrade  
    ~ $ sudo apt install <package>  
    ~ $ sudo nano <filename>  
    ~ $ sudo systemctl status <program>  
    ~ $ sudo iptables -L  
    ~ $ sudo ss -tulnp  
  
