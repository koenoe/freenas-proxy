# freenas-proxy
My FreeNAS proxy using Nginx reverse proxy to redirect traffic (80/443) to internal IPs on different ports.

## Installation guide

### Configure Nginx
Create a standard jail via the UI of FreeNAS. Call it 'webserver' and login at FreeNAS (which is 192.168.192.34 in my case) via SSH in your Terminal:
```sh
ssh root@192.168.192.34
```

Identify jail 'webserver' and login. Install Git and Nginx:
```sh
jls
jexec <ID> csh

pkg upgrade
pkg install nano git nginx
```

Open the file '/etc/rc.conf' and add following line:
```sh
nginx_enable="YES"
```

Start Nginx:
```sh
service nginx start
```

### Install SSL certificate in jail
Download the certificate at TransIP. Unzip, open a new Terminal window and point to the folder. Merge the .crt files into one:
```sh
cat certificate.crt cabundle.crt > ssl-bundle.crt
```

Secure the bundled certificate and key to the 'webserver' jail via FreeNAS:
```sh
scp ssl-bundle.crt certificate.key root@192.168.192.34:/mnt/tank/jails/webserver/usr/local/etc/nginx
```

Login again in FreeNAS via ssh and generate dhparam.pem file for extra security:
```sh
cd /usr/local/etc/nginx && openssl dhparam -out dhparam.pem 4096
```

### Clone this repository
```sh
cd /root && git clone https://github.com/koenoe/freenas-proxy.git
```

### Create a symlink for Nginx configuration
```sh
cd /usr/local/etc/nginx && mv nginx.conf nginx.conf.backup && ln -s /root/freenas-proxy/nginx.conf nginx.conf
```

### Start Nginx
Make sure the log folder exist and restart Nginx:
```sh
cd /usr/local/etc/nginx && mkdir logs
service nginx restart
```
