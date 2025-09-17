Opened the SSH configuration file:

sudo vi /etc/ssh/sshd_config

Found the Authentication section in the file (around line 714)

Typed: PermitRootLogin no


Saved and exited vi

Pressed Esc to exit insert mode
Typed :wq and pressed Enter

<img width="872" height="141" alt="image" src="https://github.com/user-attachments/assets/86c55212-fef0-4d56-b832-8d3c601655ea" />


Deliverable 2.

<img width="721" height="116" alt="image" src="https://github.com/user-attachments/assets/fe824cff-e479-4869-83b5-cace99f5b38a" />
 what prevents roots login is the 
 PermitRootLogin
