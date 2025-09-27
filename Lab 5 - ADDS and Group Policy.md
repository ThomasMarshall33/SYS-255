OU Structure Creation
* Open up Active Directory Users and Computers in server manager 
The first thing we want to do is create an organizational unit called "SYS255",& within this OU we will add child OU's for Accounts, Computers, and Groups.
* <img width="695" height="474" alt="image" src="https://github.com/user-attachments/assets/467fa50c-611b-44e2-b0db-b52c2cccadd9" />
* <img width="775" height="571" alt="image" src="https://github.com/user-attachments/assets/74b50a69-4fc1-481f-ac22-85f19c81dfb5" />

* once you have created it what you must do is right click on the SYS25 and do the same steps ---> new ---> orgaizational unit ---> create three diffrent ones the three being Accounts, Computers, and Groups.

* <img width="135" height="69" alt="image" src="https://github.com/user-attachments/assets/9da24f6a-022c-4408-ab0d-d3d6417c8b43" />

* now you have to create three users bob alice and charlie
* <img width="780" height="570" alt="image" src="https://github.com/user-attachments/assets/5f731b21-90f0-4b4b-9deb-c024a6ca0411" />
* then drag Drag WKS01 from the yourname.local\Computers Folder to the SYS255\Computers OU.  This will allow us to treat SYS255 OU Computers differently than others.
* then Within the SYS255\Groups OU, add a global security group called custom-desktop dont change any of the defualts just name it and be done
* <img width="1481" height="629" alt="image" src="https://github.com/user-attachments/assets/7749070a-269e-4c2b-a7b5-0227678cb8d2" />
* then we are gonna add alice and bob to this group but not charlie 
* <img width="781" height="579" alt="image" src="https://github.com/user-attachments/assets/db164506-c792-4cbf-8cb9-52f58f756043" />
* <img width="299" height="338" alt="image" src="https://github.com/user-attachments/assets/2e178fa7-b90d-4574-ac85-875973e31a38" />
* when you click add just type their names into the bottom text box nothing else is required
* now you can back out and goto server manager
* once there what we are going to do is create a group policy that defines some User level settings:
* <img width="774" height="576" alt="image" src="https://github.com/user-attachments/assets/4c5ee1ec-3e72-420c-9109-e0c04dd4ce0c" />
* <img width="775" height="580" alt="image" src="https://github.com/user-attachments/assets/74da989f-6ccd-4a50-ad20-65bd34e1fee9" />
*  once you click that it will ask for name just input SYS255
*  Now, this SYS255-desktop Group Policy should only apply to those users in this OU who are members of the custom-desktop security group.
* You set this using the security filters section of the group policy.  By default, All Authenticated Users have access to apply and read group policy, we will restrict this through the following steps.
* <img width="565" height="329" alt="image" src="https://github.com/user-attachments/assets/4657fc39-4a6f-4179-9376-83cb81ec8fef" />
* Step 1. Add the custom-desktop group created earlier to the Security Filter
* <img width="1474" height="571" alt="image" src="https://github.com/user-attachments/assets/4d46c88f-dbb0-4cc9-8f10-dfdd3ecdaf01" />
* Step 2. Remove Authenticated Users from the Security Filter.  
* <img width="1125" height="570" alt="image" src="https://github.com/user-attachments/assets/9805d951-eefa-4513-92d0-6821b52540f3" />
* Step 3, Add Domain Computers
* <img width="568" height="349" alt="image" src="https://github.com/user-attachments/assets/33b15bec-1255-4329-89db-61f675648111" />
* tip if you cant find something try typing the first part of the name for example I couldnt find domain computers so i just typed domain and everthing that had domain in it poped up making it much easier to fid what i was looking for
* Step 4.  Delegation tab -> Advanced (Uncheck Apply Group Policy, Select Deny)
* <img width="852" height="625" alt="image" src="https://github.com/user-attachments/assets/6030a4c0-40fc-4ccc-a7e6-59d0a84f0b3d" />
* Once we have defined who this policy applies to, we are now ready to author what the group policy does.
* <img width="947" height="713" alt="image" src="https://github.com/user-attachments/assets/2c0cddc5-4f45-42c5-a188-68164b305c98" />


* This is the bulk of the group policy editor on a Windows server where we can define computer and user settings. Remember: COMPUTER settings are applied when workstations turn on, where USER settings apply after users login. 

* There are a handful of settings here that we can define and really control the experience of the workstation in this domain. This is commonly used to control things such as, but not limited to: desktop backgrounds, browser settings, password policies, network shares, printers, redirected folders, Microsoft Bitlocker, application allowed list policies, logon scripts, etc.

* <img width="1001" height="670" alt="image" src="https://github.com/user-attachments/assets/3c55697c-b712-4a68-adeb-10855314a963" />

* <img width="1017" height="756" alt="image" src="https://github.com/user-attachments/assets/ff0cbe11-9b6d-486e-9bcb-5172e2b2e113" />

* The default display of previously logged on users is widely considered a security vulnerability, particularly in shared systems.  The next policy will turn off the default display of previously logged-in users

* <img width="947" height="714" alt="image" src="https://github.com/user-attachments/assets/47bdfa8f-cd9c-4333-86f5-2996d268ec32" />

* <img width="950" height="684" alt="image" src="https://github.com/user-attachments/assets/5e472de3-8f4d-4a04-818f-d78357d6cae9" />

* <img width="935" height="702" alt="image" src="https://github.com/user-attachments/assets/c6ef4528-3dd9-445d-913d-10a8f623344b" />

* <img width="956" height="723" alt="image" src="https://github.com/user-attachments/assets/caa6af58-cb12-4820-9660-1729385a9056" />

* 




