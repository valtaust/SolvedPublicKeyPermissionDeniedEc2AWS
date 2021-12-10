# SolvedPublicKeyPermissionDeniedEc2AWS
Ec2 Student Instance public key is approx 20 chars length compared to labsuser.pem key

https://aws.amazon.com/premiumsupport/knowledge-center/ec2-linux-fix-permission-denied-errors/

1.    touch authorized_keys command makes an empty file into .ssh/ directory) (optional) 

2.    command ssh-keygen -y (press enter)   
write in .ssh/labsuser.pem to show smaller public key that goes into edit data later if needed. (save that key to notepad). 
Paste that smaller key to authorized_keys file with vi editor. Just the key without any tags like ---begin key--- or the --end key--

3.     ssh......labsuser.pem command to try gain access first
ssh -i .ssh/labsuser.pem ubuntu@ec2-xx-xx-xx-xx.compute-1.amazonaws.com    [ec-xx-xx-xx-xx is the ip address]
chmod 400 authorized_keys if permissions still deny access.

4.    Edit the stopped machine in Actions ---> instance setting ----> edit user data like with the smaller key from the authorized key section as in the code (optional step might not be required if you have a default already or got access) from previous steps
Save the default from inside the textbox first. This is a difficult step no way back. 

Inside directory .ssh/$   I used ls command to list files. The end result is I have 2 files labsuser.pem and authorized_keys files. If there is a vockey.pem here I suggest to keep it because it is a similar to a backup private key. labsuser.pem is a private key. authorized_keys is a public key. 

For the textBox called edit user data amend this code with a copy-paste of the authorized_keys char data (about 20 characters file compared to the regular private key).
labsuser.pem key is approximately 40-50 chars in length of key. Note that in edit user data file the name of the OS_USER is the name assigned to the virtual machine. Change the name at the = operator where it states OS_USER=ubuntu.

User data below this line--------------------------------->

Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"
#cloud-config
cloud_final_modules:
- [scripts-user, always]
--//
Content-Type:
text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"
#!/bin/bash
OS_USER=ubuntu
chown root:root /home
chmod 755 /home
chmod 700 /home/$OS_USER
chmod 700 /home/$OS_USER/.ssh
chmod 600 /home/$OS_USER/.ssh/authorized_keys
echo 'ssh-rsa ENTER SMALLER KEY OUTPUTTED FROM THE SYSGEN COMMAND $OS_USER:$OS_USER/home/$OS_USER -R
--//
