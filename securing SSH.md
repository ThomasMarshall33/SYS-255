Opened the SSH configuration file:

sudo vi /etc/ssh/sshd_config

Found the Authentication section in the file (around line 714)

Typed: PermitRootLogin no


Saved and exited vi

Pressed Esc to exit insert mode
Typed :wq and pressed Enter
