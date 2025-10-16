user for account on fs
thomas-adm
passs: jack
user bob pass
b0bthefatcat123!!
user alice pass
fortnitecat123!!
* You will be prompted to setup a username and password. This is the Local Administrator for this server (and not the AD Domain Admin, since it is not joined to your AD Domain yet). Note: be sure to document the userid and password you create.
* Set the IP address settings and the Server name using the command line: sconfig.  The following screenshot shows what your network configuration and sconfig status should look like.
* MUST HAVE EVERYTHING ON TO CHANGE IP SETTING AND EVERYTHING OFF TO JOIN DOMAIN 
and when done it should look like this
<img width="518" height="498" alt="image" src="https://github.com/user-attachments/assets/07194595-c45e-4442-be82-c1fbc29be833" />
<img width="547" height="327" alt="image" src="https://github.com/user-attachments/assets/3a64e9c9-12dd-4336-bee4-afdcc6302399" />

*  once done logon to ad and do the following 
* On AD02, within the Remote Server Administration Tools (RSAT) Feature (which is not a Role), add File Service Tools and File Server Resource Manager Tools. Make sure you are logged on to ad02 as your AD Domain named -adm user.


<img width="463" height="709" alt="image" src="https://github.com/user-attachments/assets/f0a32111-9461-4b4e-b610-fd147f86791b" />
<img width="667" height="719" alt="image" src="https://github.com/user-attachments/assets/515afcde-0537-4753-9242-de66f6375287" />
<img width="664" height="458" alt="image" src="https://github.com/user-attachments/assets/d2de72c9-6cd7-4f1d-b24a-d6edb24ab3ce" />

You should remember how to create users and groups if you don't find it in sys-255 tech journal 
then once done follow these steps 
<img width="556" height="796" alt="image" src="https://github.com/user-attachments/assets/2d7adfa4-5488-4803-ac8e-cde892a69edc" />
<img width="655" height="600" alt="image" src="https://github.com/user-attachments/assets/27d8a0da-3af1-45f6-bd75-78ed65bdb629" />
<img width="658" height="398" alt="image" src="https://github.com/user-attachments/assets/74488822-8c9c-4007-8951-ed9254f6f4a6" />
<img width="695" height="422" alt="image" src="https://github.com/user-attachments/assets/f81b97bd-88f5-4cdc-90c6-3472d979299f" />
<img width="674" height="522" alt="image" src="https://github.com/user-attachments/assets/3f36ce34-a1c3-461e-bc38-914d476ad087" />
<img width="693" height="473" alt="image" src="https://github.com/user-attachments/assets/351e5492-3a20-4a5a-bc70-3803d1864cf1" />
# change permissions beofre you complete because if you dont you could run into issue where the share group keeps going back to everyone
# NOTE OF MY UNC <img width="1474" height="886" alt="image" src="https://github.com/user-attachments/assets/30614a29-a670-48f1-be02-1fb28a39d134" />
GPO For Mapping Network Drives
In the Group Policy Management tool, create a new GPO within the SYS-255 OU, and edit it. After that follow these steps 
<img width="1464" height="853" alt="image" src="https://github.com/user-attachments/assets/21cc8ed3-569e-4c35-9939-88bf22e4b0f8" />
<img width="1475" height="963" alt="image" src="https://github.com/user-attachments/assets/b50e0400-6422-411a-9328-7ba604dbcf7c" />
<img width="1332" height="825" alt="image" src="https://github.com/user-attachments/assets/b9867c94-8704-4ed3-9f81-b8e6841420a7" />
<img width="1345" height="864" alt="image" src="https://github.com/user-attachments/assets/97f7ac39-006c-4703-b36d-8afd2e960ac0" />
