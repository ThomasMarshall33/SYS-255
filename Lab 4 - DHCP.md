First, what you will need to do is download PuTTY, start up everything, then log in to ad01-thomas.marshall 

To download this program you have to go to Explorer 

To do this, go into file explorer and go to this pc, then Local Disk C 

Then the  program files(x86) and Internet Explorer will be there 

Look up PuTTY and click the link that is by chiark 

Download the top link. This should be called something like

putty-64bit - 0.83-installer.msi 

Now just follow the defaults for PuTTY

Once you have downloaded it you will need to open PuTTY

To do this you are gonna follow the same steps to open Internet Explorer, but instead of Program Files (x86), go into just Program Files

And double-click PuTTY and it should be in there 

then once PuTTY is installed(i will be refreing to it as pu from now on for ease) type the host name as dhcp01-thomas 

once in pu ype sudo -i 

then cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak
then vi/etc/dhcp/dhcpd.cof
then enter all of this exactly how the image is 


<img width="906" height="637" alt="image" src="https://github.com/user-attachments/assets/38898c50-0d8d-444f-8e04-8b3c2d8a45c5" />
once inputed all info press esc then type :wq 

cat /etc/dhcp/dhcpd.conf
after that command do these two 
systemctl start dhcpd
systemctl enable dhcpd 
enabled it on startup which you should do 


<img width="813" height="606" alt="image" src="https://github.com/user-attachments/assets/6c274166-1a70-42f1-83f7-3d44525aeaf2" />


now you Configuring the Firewall to allow incoming DHCP requests
to do this you must type these commands 
<img width="639" height="90" alt="image" src="https://github.com/user-attachments/assets/22cbaf17-1fb0-4b6a-a888-63e8bc0fc13e" />
once done run these commands 
<img width="401" height="190" alt="image" src="https://github.com/user-attachments/assets/f02a339c-db50-49ab-abf9-1e0e59a24c1c" />


after you finsih this goto wks01 and change the ethernet 
should look like this 
<img width="1398" height="996" alt="image" src="https://github.com/user-attachments/assets/34143897-8a69-46cf-a5c2-abb0c40dcebb" />






What is PuTTY
PuTTY is a free SSH and Telnet client for Windows. It's a terminal emulator that allows you to:

Connect remotely to servers and network devices
Access command-line interfaces on Linux/Unix systems
Manage routers, switches, and other network equipment
Execute commands on remote systems securely

It's commonly used by network administrators and IT professionals for remote system management and configuration.


