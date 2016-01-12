# freenas-proxy
My FreeNAS proxy using Nginx reverse proxy to redirect traffic (80/443) to internal IPs on different ports.

## Installation guide

### Configure Nginx
Create a standard jail via the UI of FreeNAS. Call it 'webserver' and login at FreeNAS via SSH.

Identify jail 'webserver' and login. Install Git and Nginx:
``
jls
jexec <ID> csh

pkg upgrade
pkg install nano
pkg install git
pkg install nginx
``

Open the file '/etc/rc.conf' and add following line:
``
nginx_enable="YES"
``

Start Nginx:
``
service nginx start
``

### Install SSL certificate in jail
Download the certificate at TransIP. Unzip, open Terminal and point to the folder. Merge the .crt files into one:
``
cat cabundle.crt certificate.crt > ssl-bundle.crt
``

Secure the bundled certificate and key to the 'webserver' jail via FreeNAS (which is 192.168.192.34 in my case)
``
scp ssl-bundle.crt root@192.168.192.34:/mnt/tank/jails/webserver/usr/local/etc/nginx
scp certificate.key root@192.168.192.34:/mnt/tank/jails/webserver/usr/local/etc/nginx
``

### Clone this repository
Blabla

### Create a symlink in /etc/nginx/sites-enabled to point to our vhost
Blabla
