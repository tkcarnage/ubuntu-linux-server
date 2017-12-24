# Linux Server Configuration
Directions on how to run Lightsail ubuntu server to serve ItemCataApp web application.

### See it working
[34.210.73.97](http://34.210.73.97/)

## Step by Step Directions
1. Create Lightsail instance. [Lightsail](https://lightsail.aws.amazon.com/ls/webapp)
1. Connect to the instance using the Lightsail website, it should be a orange button saying "Connect using SSH"
1. Update the server running these two commands
  1. `sudo apt-get update`
  1. `sudo apt-get upgrade`
1. Create new user, grader, with sudo permissions
  1. `sudo adduser grader`
  1. `sudo usermod -aG sudo grader`
1. Login from your own terminal
  1. Generate ssh key pair
    1. Run `ssh-keygen` command on your own machine and save it
  1. On your virtual machine switch to grader and create .ssh directory and authorized_keys file
    1. `su - grader`
    1. `sudo mkdir .ssh`
    1. `sudo touch .ssh/authorized_keys`
  1. Open authorized_keys file and copy and paste the .pub key you generated from your local file and save.
    1. `sudo nano .ssh/authorized_keys`
  1. Give the correct permissions to the .ssh dir and authorized_keys file
    1. `sudo chmod 700 .ssh`
    1. `sudo chmod 644 .ssh/authorized_keys`
  1. Restart by running `service ssh restart`
  1. Quit and login using new keys
    1. `ssh grader@34.210.73.97 -i ~/.ssh/"privatekey"`
1. Configure the Uncomplicated Firewall (UFW)
  1. First go to networking tab in the instance on Lightsail to add custom Firewall rule
    1. TCP at port 2200 **DO THIS BEFORE NEXT STEP OR YOU WILL GET LOCKED OUT FROM YOUR INSTANCE**
  1. Go to the sshd_config file and change the ssh port to 2200
    1. `sudo nano /etc/ssh/sshd_config` edit the port number from 22 to 2200
  1. Now configure the rest of the UFW
    1. `sudo ufw default deny incoming`
    1. `sudo ufw default allow outgoing`
    1. `sudo ufw allow 2200/tcp`
    1. `sudo ufw allow www`
    1. `sudo ufw allow ntp`
    1. `sudo ufw enable`
    1. Check the rules using `sudo ufw status`
1. Disable Root login
  1. Go to sshd_config and disable PermitRootLogin
    1. `sudo nano /etc/ssh/sshd_config` PermitRootLogin no
1. Configure local timezone to UTC
  1. `sudo dpkg-reconfigure tzdata` use GUI to set it to UTC
1. Install Apache and Git
  1. Install Apache using `sudo apt-get install apache2`
  1. Install mod_wsgi `sudo apt-get install libapache2-mod-wsgi-py3`
  1. Install git `sudo apt-get install git`
1. Serve the Web application
  1. 
