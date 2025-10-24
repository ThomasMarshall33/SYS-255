First to join ad you must install the realmd service.
then what you must do is type the folliwing command 

<img width="545" height="299" alt="image" src="https://github.com/user-attachments/assets/8ddcf5df-ff7a-4f4c-b32a-4dd4d078432c" />
must type thomas.local as all uppercase


sudo dnf install httpd
This installs the Apache HTTP Server package on Rocky Linux.

Step 2: Start and Enable the Apache Service
bashsudo systemctl enable httpd
sudo systemctl start httpd

enable: Configures Apache to start automatically on boot
start: Starts the Apache service immediately


Step 3: Verify Apache is Running
bashsudo systemctl status httpd
You should see "active (running)" in green.

Firewall Configuration with firewall-cmd
Step 4: Add HTTP Service to Firewall
bashsudo firewall-cmd --permanent --add-service=http
This opens port 80 for HTTP traffic.

Step 5: Add HTTPS Service to Firewall
bashsudo firewall-cmd --permanent --add-service=https
This opens port 443 for HTTPS traffic.

Step 6: Reload Firewall to Apply Changes
bashsudo firewall-cmd --reload
This applies the firewall rules immediately.

Step 7: Verify Firewall Configuration
bashsudo firewall-cmd --list-all
Expected output should include:

services: ssh http https
ports: 80/tcp 443/tcp


Testing Apache
Step 8: Test Web Server Access
From a web browser on another machine, navigate to:
http://your-server-hostname
or
http://your-server-ip
You should see the default Apache test page.

Creating a Custom Web Page
Step 9: Remove Default Welcome Page
bashsudo rm -f /etc/httpd/conf.d/welcome.conf

Step 10: Create Custom index.html
bashsudo vi /var/www/html/index.html
Add your content:
html<h1>Welcome to web01-yourname</h1>
<p>Apache HTTP Server is running!</p>
Save and exit (:wq in vi).

Step 11: Test Custom Page
Refresh your browser - you should now see your custom page.

Important firewall-cmd Commands
Check Current Firewall Status
bashsudo firewall-cmd --state
List All Services
bashsudo firewall-cmd --get-services
Remove a Service
bashsudo firewall-cmd --permanent --remove-service=http
sudo firewall-cmd --reload
Add a Specific Port
bashsudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
Check if Service is Enabled
bashsudo firewall-cmd --query-service=http

Troubleshooting
Apache Won't Start
Check error logs:
bashsudo journalctl -u httpd -n 50
Can't Access Website

Verify Apache is running: sudo systemctl status httpd
Check firewall: sudo firewall-cmd --list-all
Verify network connectivity: ping server-ip

Firewall Changes Not Working
Make sure to reload after making changes:
bashsudo firewall-cmd --reload

Summary
Apache Installation:

sudo dnf install httpd
sudo systemctl enable httpd
sudo systemctl start httpd

Firewall Configuration:

sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
sudo firewall-cmd --list-all (verify)
