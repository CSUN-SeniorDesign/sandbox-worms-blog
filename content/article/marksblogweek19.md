---
title: "Mark's Blog Week 19"
date: 2019-02-015T120:21:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 19]
Author: "Mark Siegmund"
---

Marks Blog Week 19					February 15, 2019
Prep for DHCP - file list
Prep for DNS   - file list
Prep for Firewall - file list
Prep for VPN - file list
NTP Primary 10.47.0.2
NTP Secondary 10.47.0.3
This week was the first week as our newly formed group of 4 members.  I thought we were ready to make the transition to the new servers at the end of last week and that it would be done early this week.  It is still not done.  Files have been identified
We need to identify all files to be backed up.
For DHCP is is pretty simple.
  /etc/dhcp/dhcpd.conf         - main file must keep updated….
  /etc/dhcp/dhclient.conf      - not important but may provide some info
 /etc/dhcp/debug                 - only used for troubleshooting
 /etc/dhcp/dhcpd6.conf        - only needed for IPv6 support

This week I found in the dhcpd conf file a reference to two NTP servers.  As one might guess Jeff made them the same as FWA and FWB.  A is the primary and B is the secondary.  This is one more service that we need to check for.

For DNS to make it easy we are just going to back up the entire  
/etc/bind
But the key files are 
/etc/bind/named.conf      - main file
/etc/bind/named.conf.local
/etc/bind/named.conf.options
/etc/bind/db.(anything)
/etc/bind/zones/*.*   - all of these are important to get copies

Th VPN info is provided by Mario and Thomas

I spent my week on getting the iLo to be configured and working.  When we set them up before the install in the MDF I set the passwords to the default passwords on case.  This needs to be set to the standard password for the techlab account.  At the time of configuring the iLo interface I did not know what interface they were going to use.  During the writing of the documentation Nick found that all the iLo ports used the 10.47.0.x  .  The even IPs  are all in Rack Y12 and all the odd IPs are in rack Y13. 
To get into iLo setup press F8 key to force the new servers to show the configuration.  If this is not done the setup will show nothing and just go by.  *** It takes the new servers about 2-4 minutes to turn on and start the boot process.

 I decided to use
10.47.0.200 for the Y12 rack but we had problems making this work. 
I set 10.47.0.201 for the Y13 rack.
 I spent some time figuring out that when we SSH to the iLo port that it uses a deprecated algorithm to generate keys called DSA.  DSA has 1024 digits at max for encryption and that is not good enough for today's encryption.  The iLo port is older and need to use DSA.  DSS is what the key is called after the algorithm has generated the key.
When useing the standard ssh 10.47.0.200 we got
SSH returns: no matching host key type found
The solution is to use :     ssh -oHostKeyAlgorithms=+ssh-dss administrator@10.47.0.200




I also spent time researching openVPN and how to generate keys.  
I updated openVPN….. 

The stpes I followed were
Step 1- Install / update open VPN
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install openvpn easy-rsa

Step2 - set up the certificate authority
We can utilize the easy-rsa template by copying it to a new directory, and then entering that directory to move into the configuration.

$ make-cadir ~/openvpn-ca
$ cd ~/openvpn-ca

We need to edit some of the variables that help decide how to create the certificates. Use nano—or another favorite editor—to open the file. We’ll be editing some variables toward the end of the file.

$ nano vars
Look for the section below—the easy-rsa template provides some default fields for these variables, but you should change them according to your needs. Make sure you also change the KEY_NAME variable as well. It’s not so important what you change these to, rather that you don’t leave them in the default state, or blank.

# These are the default values for fields
# which will be placed in the certificate.
# Don't leave any of these fields blank.
export KEY_COUNTRY="US"
export KEY_PROVINCE="CA"
export KEY_CITY="SanFrancisco"
export KEY_ORG="Fort-Funston"
export KEY_EMAIL= class="hljs-string">"me@myhost.mydomain"
export KEY_OU="MyOrganizationalUnit"

# X509 Subject Field
export KEY_NAME="EasyRSA"
After some tweaks:

# These are the default values for fields
# which will be placed in the certificate.
# Don't leave any of these fields blank.
export KEY_COUNTRY="US"
export KEY_PROVINCE="CA"
export KEY_CITY="Tustin"
export KEY_ORG="SSD Nodes"
export KEY_EMAIL= class="hljs-string">"joel@example.com"
export KEY_OU="Marketing"

# X509 Subject Field
export KEY_NAME="vpnserver"
Now, source the vars file you just edited. If there aren’t any errors, you’ll see the following output.

$ source vars
NOTE: If you run ./clean-all, I will be doing a rm -rf on /home/user/openvpn-ca/keys
Now we can clean up the environment and then build up our CA.

$ ./clean-all
$ ./build-ca
A new RSA key will be created, and you’ll be asked to confirm the details you entered into the vars file earlier. Just hit Enter to confirm.


Step 3 Create the server public/private keys 
When you run the below command you can change [server] to the name of your choice. Later, you’ll need to reference this name. For the sake of this tutorial, we’re choosing with vpnserver.

Note: When prompted, do not enter a password.

Finally, you’ll be asked two questions about signing the certificate and committing it. Hit y and then Enter for both, and you’ll be done.

$ ./build-key-server [server]
Next, you need to build Diffie-Hellman keys.

$ ./build-dh
Finally, you need to generate an HMAC signature to strengthen the certificate.

$ openvpn --genkey --secret keys/ta.key


Step 4 - Create the client public/private keys

This process will create a single client key and certificate. If you have multiple users, you’ll want to create multiple pairs.

When running the below command, hit Enter to confirm the variables we set and then leave the password field blank.

$ source vars
$ ./build-key client1
If you want to create password-protected credentials, use build-key-pass instead:

$ source vars
$ ./build-key-pass client1


Step 5 - Configure openVPN server
First, you need to copy the keyfiles we created in ~/openvpn-ca into the /etc/openvpn directory. Note: change the vpnserver.crt and vpnserver.key files according to the [server] name you chose earlier.

$ cd ~/openvpn-ca/keys
$ sudo cp ca.crt ca.key vpnserver.crt vpnserver.key ta.key dh2048.pem /etc/openvpn
Now, extract a sample OpenVPN configuration to the default location.

$ gunzip -c /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz | sudo tee /etc/openvpn/server.conf
We now need to make some edits to the configuration file.

$ sudo nano /etc/openvpn/server.conf
First, let’s ensure that OpenVPN is looking for the right .crt and .key files.

Before:

ca ca.crt
cert server.crt
key server.key  # This file should be kept secret
After (change according to the [server] name you chose earlier):

ca ca.crt
cert vpnserver.crt
key vpnserver.key  # This file should be kept secret
Next, enforce identical HMAC between clients and the server.

Before:

;tls-auth ta.key 0 # This file is secret
After:

tls-auth ta.key 0 # This file is secret
key-direction 0
Because we are going to use this VPN to route our traffic to the internet, we need to uncomment a few lines&nbsp;to help us establish DNS. You should also remove bypass-dhcp from the first line in question.

If you would prefer to use a DNS other than opendns, you should change the two lines that begin with push "dhcp-option.

Before:

# If enabled, this directive will configure
# all clients to redirect their default
# network gateway through the VPN, causing
# all IP traffic such as web browsing and
# and DNS lookups to go through the VPN
# (The OpenVPN server machine may need to NAT
# or bridge the TUN/TAP interface to the internet
# in order for this to work properly).
;push "redirect-gateway def1 bypass-dhcp"

# Certain Windows-specific network settings
# can be pushed to clients, such as DNS
# or WINS server addresses.  CAVEAT:
# http://openvpn.net/faq.html#dhcpcaveats
# The addresses below refer to the public
# DNS servers provided by opendns.com.
;push "dhcp-option DNS 208.67.222.222"
;push "dhcp-option DNS 208.67.220.220"
After:

# If enabled, this directive will configure
# all clients to redirect their default
# network gateway through the VPN, causing
# all IP traffic such as web browsing and
# and DNS lookups to go through the VPN
# (The OpenVPN server machine may need to NAT
# or bridge the TUN/TAP interface to the internet
# in order for this to work properly).
push "redirect-gateway def1"

# Certain Windows-specific network settings
# can be pushed to clients, such as DNS
# or WINS server addresses.  CAVEAT:
# http://openvpn.net/faq.html#dhcpcaveats
# The addresses below refer to the public
# DNS servers provided by opendns.com.
push "dhcp-option DNS 208.67.222.222"
push "dhcp-option DNS 208.67.220.220"
Then we need to select the ciphers to use. Uncomment the AES cipher and change it to 256, and then add auth SHA512 at the bottom of the block.

Before:

# Select a cryptographic cipher.
# This config item must be copied to
# the client config file as well.
;cipher BF-CBC        # Blowfish (default)
;cipher AES-128-CBC   # AES
;cipher DES-EDE3-CBC  # Triple-DES
After:

# Select a cryptographic cipher.
# This config item must be copied to
# the client config file as well.
;cipher BF-CBC        # Blowfish (default)
cipher AES-256-CBC   # AES
;cipher DES-EDE3-CBC  # Triple-DES
auth SHA512
Finally, let’s have OpenVPN use a non-privileged user account instead of root, which isn’t particularly secure.

user openvpn
group nogroup
You can now save and close this file in order to create that user:

$ sudo adduser --system --shell /usr/sbin/nologin --no-create-home openvpn
The OpenVPN server should now be set up!

Step 6 - Start up the VPN server.
Before we configure our clients, let’s make sure the OpenVPN server is running as we hope it will.

Make sure to turn on TUN/TAP in the SSD Nodes dashboard.

$ sudo systemctl enable openvpn@server
$ sudo systemctl start openvpn@server
You can double-check that OpenVPN is running with the systemctl status command:

$ sudo systemctl status openvpn@server
If you’re having problems getting OpenVPN to start, commenting out the LimitNPROC in /lib/systemd/system/openvpn@.service, as discovered in this Ask Ubuntu thread may be useful. You’ll then need to run sudo systemctl daemon-reload and then sudo systemctl start openvpn@server.

You will also need to set up iptables to properly direct traffic. First, look for the default interface.

$ sudo ip route | grep default
default dev venet0  scope link
The venet0 field is what we’re looking for. And then we set up iptables. In order to ensure this rule is persistent between reboots, isntall the iptables-persistent package, which will prompt you to save existing rules. Choose Yes and your rules will be persisted movign forward.

$ sudo iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o venet0 -j MASQUERADE
$ sudo apt-get install iptables-persistent

Step7 Configure clients
Lastly, you need to create client configurations. You can store these in any folder you’d like—they don’t need to be kept secret—as long as it isn’t the /etc/openvpn folder. We’ll create a directory in home for this purpose.

$ cd ~
$ mkdir openvpn-clients
cd openvpn-clients
Now, copy the sample client configuration into this new directory, and then open it in nano for editing.

$ cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf ~/openvpn-clients/base.conf
$ nano base.conf
Look for the following block of lines. You’ll need to change the my-server-1 to the public IP address of this VPS. You can find this information in the SSD Nodes dashboard, or by typing in the ifconfig command and looking for the inet field that does not look like 127.0.0.x.

# The hostname/IP and port of the server.
# You can have multiple remote entries
# to load balance between the servers.
remote my-server-1 1194
;remote my-server-2 1194
Next, uncomment the following two lines by removing the semicolon.

Before:

# Downgrade privileges after initialization (non-Windows only)
;user nobody
;group nogroup
After:

# Downgrade privileges after initialization (non-Windows only)
user nobody
group nogroup
Because we’ll be adding keys and certificates directly into the .ovpn file, let’s comment out the following lines by adding semicolons to the beginning.

Before:

# SSL/TLS parms.
# See the server config file for more
# description.  It's best to use
# a separate .crt/.key file pair
# for each client.  A single ca
# file can be used for all clients.
ca ca.crt
cert client.crt
key client.key
After:

# SSL/TLS parms.
# See the server config file for more
# description.  It's best to use
# a separate .crt/.key file pair
# for each client.  A single ca
# file can be used for all clients.
;ca ca.crt
;cert client.crt
;key client.key
Finally, jump to the bottom of the file and add the following lines. The first two mirror the cipher/auth options we added to the server.conf file earlier, and the third establishes that this files will be used to connect to the server, not the other way around.

We’re also adding three commented-out files that should be uncommented for Linux-based systems that use update-resolv-conf.

# Added lines via SSD Nodes tutorial
cipher AES-256-CBC
auth SHA512
key-direction 1

# script-security 2
# up /etc/openvpn/update-resolv-conf
# down /etc/openvpn/update-resolv-conf
Finally, you need to embed the keys and certificates into an .ovpn file using base.conf as a framework. Copy this entire command and execute it to embed the keys and create a final client1.ovpn file.

$ cat base.conf 
  <(echo -e '<ca>') ~/openvpn-ca/keys/ca.crt <(echo -e '</ca>') 
  <(echo -e '<cert>') ~/openvpn-ca/keys/client1.crt <(echo -e '</cert>n') 
  <(echo -e '<key>') ~/openvpn-ca/keys/client1.key <(echo -e '</key>n') 
  <(echo -e '<tls-auth>') ~/openvpn-ca/keys/ta.key <(echo -e '</tls-auth>') 
  >> client1.ovpn
This tutorial won’t cover client configurations in detail, but we’ll share one easy way to transfer the .ovpn file to your Linux or OS X client. This command will ssh into your VPS, and then use cat to write a new client1.ovpn file on your local machine.

$ ssh USER@SERVER-IP "cat ~/openvpn-clients/client1.ovpn" > client1.ovpn
Once you configure your client, you should be able to connect to the VPN and access the wider internet through it. 


We identified the moodle.csun.edu box.  It is in rack A or Y12.  There is no iLo port for the Moodle machine.  It looks like a regular PC box mounted in the rack.



