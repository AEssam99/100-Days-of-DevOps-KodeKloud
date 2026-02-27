# Day 15: Setup SSL for Nginx
## Task 
The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 3 in Stratos Datacenter. 
They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:

1. Install and configure nginx on App Server 3.


2. On App Server 3 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. 
Move them to some appropriate location and deploy the same in Nginx.


3. Create an index.html file with content Welcome! under Nginx document root.


4. For final testing try to access the App Server 3 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://app-server-ip/.

## Solution
### Install and Configure Nginx
```sh
sudo yum install -y nginx
```
Enable and start nginx:
```sh
sudo systemctl enable --now nginx
```
Check Status:
```
sudo systemctl status nginx
```
### Move SSL Certificate & Configure HTTPS
Move certificate and key to proper location
```sh
mkdir -p /etc/nginx/ssl/
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```
Set proper permissions:
```sh
sudo chmod 600 /etc/nginx/ssl/nautilus.key
sudo chmod 644 /etc/nginx/ssl/nautilus.crt
```
### Configure Nginx for SSL
Edit nginx config:
```sh
sudo vi /etc/nginx/conf.d/ssl.conf
```
Add this:
```html
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Save and exit.
### Create index.html
Create file:
```sh
sudo echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```
### Restart and Allow HTTPS

Test nginx configuration:
```sh
sudo nginx -t
```
Restart nginx:
```sh
sudo systemctl restart nginx
```
### Check firewall status
```sh
sudo systemctl is-active firewalld
sudo systemctl is-active iptables
```
Both:
```sh
inactive
```
### Testing from Jump host
```sh
curl -Ik https://172.16.238.12/
curl -Ik https://stapp03/
```
`Output`
```html
HTTP/1.1 200 OK
Server: nginx
....
```
