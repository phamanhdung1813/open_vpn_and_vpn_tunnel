# OpenVPN and VPN tunnels for DNS SEVER, APACHE LINUX WEBSERVICE AND WINDOW IIS

![Diagram](https://user-images.githubusercontent.com/71564211/139771535-af0a736f-1849-4ed7-b501-d42a88b067af.JPG)

## Open VPN AND VPN TUNNEL

1. On your CentOS machine install these packages
* yum install epel-release
*	yum install openvpn

-> Then confirm the openvpn  
*	/etc/openvpn/server
*	/etc/openvpn/client

2. Generate the VPN key on your CentOS machine
* openvpn –keysize 128 –genkey –secret static.key

3. Copy configuration files and VPN key

![client](https://user-images.githubusercontent.com/71564211/139771778-b768757c-b143-4e04-a826-934948fee199.JPG)

4. Persistent VPN after reboot machine

### Using the scripts above vpnstart-C.sh (The scirpt is on conf-scripts)
### Edit crontab to manage the vpn startup file

* scp vpnstart-C.sh 192.168.26.129:/usr/scripts/
* sudo chmod +x /usr/scripts/vpnstart-C.sh
* sudo bash /usr/scripts/vpnstart-C.sh

![vpn](https://user-images.githubusercontent.com/71564211/139772112-06bb7578-7dcc-404b-b300-b000a4c87316.PNG)

5. REPEAT THESE STEP ABOVE FOR THE ROUTER CENTOS ON AZURE 
* sudo yum install epel-release
* sudo yum install openvpn
* sudo mkdir /usr/scripts
* scp server.conf az-fer21347477.canadacentral.cloudapp.azure.com:/etc/openvpn/server/
* scp vpnstart-S.sh az-fer21347477.canadacentral.cloudapp.azure.com:/usr/scripts/
* scp static.key ssh az-fer21347477.canadacentral.cloudapp.azure.com:/etc/openvpn/server/
* sudo chmod +x /usr/scripts/vpnstart-S.sh
* sudo bash /usr/scripts/vpnstart-S.sh

6. Configuring IPTABLES-SERVICE for AZURE CENTOS ROUTER to allow RDP from your AZURE WINDOW CLIENT
* scp iptables-tun.sh az-fer21347477.canadacentral.cloudapp.azure.com:
* sudo iptables -I FORWARD 5 -p tcp --dports 80 -j ACCEPT
* sudo iptables -I FORWARD 6 -p tcp --sports 80 -j ACCEPT
* sudo iptables -I FORWARD 7 -p tcp -m multiport --dports 21,20,9990:10000 -j ACCEPT
* sudo iptables -I FORWARD 8 -p tcp -m multiport --sports 21,20,9990:10000 -j ACCEPT
* sudo service iptables save

![az-fer ipt](https://user-images.githubusercontent.com/71564211/139772337-189ad1e1-a5c9-4a1b-a37e-71ff862a3136.JPG)

![azi-fer tnat](https://user-images.githubusercontent.com/71564211/139772340-9970923d-7b82-4373-8998-337ac1a3934d.JPG)

## DNS SERVICE FOR AZURE WINDOW CLIENT
### ASSIGN YOUR IP ON IPv4 properties
### THEN INSTALL THE DNS SERVICE ON WINDOW SERVER AND MODIFY THE DHCP TO ALLOW FORWARDER CONNECTION TO OUTSITE
 
![ipv4](https://user-images.githubusercontent.com/71564211/139772466-b1802cf4-3723-4b22-b3b6-8a4daad49f9f.JPG)

![DNS ip](https://user-images.githubusercontent.com/71564211/139772621-5219aabd-03f7-4c2c-a9f1-7639fd24f1ea.JPG)

### sudo vim /etc/dhcp/dhcpd.conf
### And fix option domain-name-servers (YOUR WINDOW AZURE CLIENT IP)

![dhcpd](https://user-images.githubusercontent.com/71564211/139772710-fbec7350-181c-4422-b5cf-05bfb66fc4c0.JPG)

### TESTING CONNECTION ON YOUR LOCAL MACHINE

![window DNS](https://user-images.githubusercontent.com/71564211/139772767-d6c5d835-470d-46fb-80b9-9b04d19caa2f.JPG)

![window server](https://user-images.githubusercontent.com/71564211/139772814-d487946f-ffba-4fed-8639-d33807e97075.JPG)

# INSTALLING FTP SERVER ON WINDOW AZURE CLIENT
### Execute the command 
* ftp://YOUR_WINDOW_FTP_DOMAINDNAME/
![ftpget](https://user-images.githubusercontent.com/71564211/139772946-402a0ecf-f16e-44c4-9dac-565389f25e14.JPG)

# BEFORE NEXT STEP, WE HAVE TO VERIFY THE NO OUTSIDE VPN AND SSH CONFIGURED

![lsssh](https://user-images.githubusercontent.com/71564211/139773072-ae4b8a65-b2b0-465a-999b-b291f1427c66.JPG)

# APACHE WEBSERVER ON LINUX MACHINE
* sudo yum install httpd
* sudo systemctl enable httpd
* sudo systemctl start httpd
* sudo touch /var/www/html/index.html
* sudo vi /var/www/html/index.html

-> GENERATE THE BASIC TEMPLATE HTML TO TEST YOUR WEB SERVER, THEN COPY THEM INTO index.html

# VERIFY THE CONNECTION ON YOUR LOCAL AND YOUR LOCAL UBUNTU MACHINE
![Linux DNS](https://user-images.githubusercontent.com/71564211/139773326-de64ef5f-adef-4603-a28f-08c2c3e2f567.JPG)

![linux server](https://user-images.githubusercontent.com/71564211/139773264-df44b901-bca6-4af0-a0b8-ec8c323480ed.JPG)

# ALLOW TRAFFIC ON SPECIFIC IPADDRES

![az-lsiptable](https://user-images.githubusercontent.com/71564211/139773440-79dd6ce1-ccbd-40c4-b4b9-4a0309f6e595.JPG)



