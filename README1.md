# Creating a Linux virtual machine (VM) instance on Microsoft Azure using SSH keys and host files on HTTP server -->
# Steps:
# 1. Register and create a VM instance
I. First of all open your account on Microsoft azure.

II. Create a virtual machine instance on azure, while creating it select your desirable and required options.

In Basic information section, we choose Ubuntu and SSH options as we are creating a Linux virtual machine instance using SSH keys.
Public IP address of VM is 104.208.107.167
After clicking the create option SSH key will be created and downloaded.

# 2. Configure SSH keys for secure access to the VM
Two ways to configure SSH keys:

I. First one is while creating VM instance, You will have to enable the SSH option and disable password option. Also select the option “generate the pair of keys”.


II. If you don’t have pair of SSH key already, You can generate a new pair on your local machine using this command: ssh-keygen

Now you have two keys public key and private key. public key will be saved in ~/.ssh/ folder by default. After that you will copy the content of the public key to the VM’s “authorized_keys” file using this command: ssh-copy-id USERNAME@VM-IP-ADDRESS
Now to enforce key-based authentication you will have to disable the password authentication, for this you will open “sshd_config” file using command: sudo nano /etc/ssh/sshd_config , on the VM.
# 3. Access the VM using SSH
After that, in connect section we will copy the following command:-

Command: ssh -i “C:\Users\username\.ssh\SSH_KEY.pem” USERNAME@VM-IP-ADDRESS
Copy the command provided in connect section, and run it on windows Power-Shell.
Path in that command should be like this, “C\Users\Username\.ssh\ssh_key”.
In this way, through SSH key we will access VM.

Command to your VM

connect to VM using SSH key
# 4. Add custom firewall rules in the subnet’s security list
After configuring and accessing SSH keys, we will setup the custom firewall rules in the subnets security rules.

I. Go to the networking section, here you can see Inbound and outbound port rules and then enter the “Effective Security Rules”, then click on the “Network Interface” to add the Inbound and outbound port rules.

II. Here we have added the inbound rule for RDP connection using service RDP and also added one more rule for HTTP connection using service HTTP.

III. After adding them, save all these security rules.


# 5. Install RDP on the VM and access it using RDP from your PC
Download RDP on VM.

I. By using command on the VM, we will download the RDP in our machine.

First of all do all updates, using command: sudo apt-get update
Then install RDP using command: sudo apt-get install xrdp
II. After downloading the RDP, we will set the username and password for the RDP machine.

III. Then go to the network interface to add inbound port rule for RDP.


# 6. Host files using a simple HTTP server and access them using the public IP
To host files on the web server, connect VM to RDP or SSH and then install Apache web server software using command: sudo apt-get install apache2


Apache Installation
I. After the installation completes, start the web server services:

Using command: sudo systemctl start apache2

II. On Apache on a Debian-based system, you can modify the default virtual host configuration file having path (/etc/apache2/sites-available/000-default.conf).

III. After that add following lines in the “apache2.conf” file:

DocumentRoot /var/www/html

<Directory /var/www/html>

Options Indexes FollowSymLinks MultiViews

AllowOverride All

Require all granted

</Directory>

IV. After updating the “apache2.conf” file, we will create files in the following path /var/www/html

V. I have created sample.txt and Test.html files.

VI. Then we will access the files using VM public IP address.

Conclusion:
This is how we will create a Linux VM’s instance on a cloud platform named Microsoft Azure using SSH keys. And hosting files over the HTTP server and accessing them using VM’s public IP address.
