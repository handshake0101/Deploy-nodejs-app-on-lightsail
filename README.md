
# Deploy-nodejs-app-on-lightsail

Create instance on Lightsail with Nodejs;  
Create static IP (Networking  > Create static IP);  
Create DNS zone (Networking  > Create DNS zone);  
Connecting using SSH;  
Go to htdocs folder:  
`cd htdocs/`  
Delete all subdirectory:  
`rm -rf *`   
Clone repo of projects:  
`git clone <link>`   
Go to new subfolder:  
`cd <repo-name>`   
Install modules:  
`npm i`   
Start app and chek IP:  
`npm start`   
Configure Apache server port (80: http): 
`vi /opt/bitnami/apache/conf/vhosts/myapp-http-vhost.conf`   
press `i` to insert code, paste this code: 
```xml
<VirtualHost _default_:80>
    ServerAlias *
    DocumentRoot "/home/bitnami/htdocs/<repo-name>"
    <Directory "/home/bitnami/htdocs/<repo-name>">
      Require all granted
    </Directory>
    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/
</VirtualHost>
```    
Warning: change `<repo-name>`.   
Press `esc`, type `:w!`, type `q`.   
Configure Apache server port (443: https):  
`vi /opt/bitnami/apache/conf/vhosts/myapp-https-vhost.conf`   
Press `i` to insert code, paste this code: 
```xml
<VirtualHost _default_:443>
    ServerAlias *
    SSLEngine on
    SSLCertificateFile "/opt/bitnami/apache/conf/bitnami/certs/server.crt"
    SSLCertificateKeyFile "/opt/bitnami/apache/conf/bitnami/certs/server.key"
    DocumentRoot "/home/bitnami/htdocs/<repo-name>"
    <Directory "/home/bitnami/htdocs/<repo-name>">
      Require all granted
    </Directory>
    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/
</VirtualHost>
```    
Warning: change `<repo-name>`.   
Press `esc`, type `:w!`, type `q`.   
Restart Apache:   
`sudo /opt/bitnami/ctlscript.sh restart apache`    
Get SSL certificate with    
`sudo /opt/bitnami/bncert-tool`   
Insert your domain.   
Finally, start your app:   
`forever start app.js`   
