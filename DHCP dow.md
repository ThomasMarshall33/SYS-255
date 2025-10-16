* All you have to do is enter your ad
* Then go to Server Manager and add roles and features wizard 
* Select the FS server and add the role DHCP Server 
* Just keep going and install 
* Then click the flag and complete the DHCP config 
* Click Next on everything and close, then open the DHCP management console 
* Right-click on it and open the DHCP Manager, and add a scope 
* Do the start and end IP
* do /24 subnet (255.255.255.0)
* Don't do exclusion, then set the duration of the lease, then you will have to add the default gateway 10.0.5.2
* Then click next all the way through and activate the scope 
* Then enable DHCP on fs01
Or if that's too confusing, follow this awesome YouTube tutorial 
https://www.youtube.com/watch?v=TM4Mnv0RMiQ

<img width="1068" height="662" alt="image" src="https://github.com/user-attachments/assets/eb14b10a-a2d1-454d-9b0b-e93e38b1b75c" />
