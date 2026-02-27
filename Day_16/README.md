# Day 16: Install and Configure Nginx as an LBR
## Task 
Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:

a. Install nginx on LBR (load balancer) server.

b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.

d. Once done, you can access the website using StaticApp button on the top bar.

## Solution
### First Let's Explain what is the LBR
*LBR = Load Balancer / Load Balancing Router (or Resource)*

An LBR server is a server (or device) that distributes incoming network traffic across multiple backend servers to:

- Improve performance
- Increase availability
- Prevent overload on a single server
- Ensure high availability (HA)
### HTTP load balancer
#### Introduction
Load balancing across multiple application instances is a commonly used technique for optimizing resource utilization, maximizing throughput, reducing latency, and ensuring fault-tolerant configurations.

It is possible to use nginx as a very efficient HTTP load balancer to distribute traffic to several application servers and to improve performance, scalability and reliability of web applications with nginx.

#### Load balancing methods
The following load balancing mechanisms (or methods) are supported in nginx:

- round-robin (Default): requests to the application servers are distributed in a round-robin fashion,
- least-connected: next request is assigned to the server with the least number of active connections,
- ip-hash: a hash-function is used to determine what server should be selected for the next request (based on the client’s IP address).
## 
### Install Nginx on the LBR server
On the LBR server
```sh 
sudo dnf install nginx -y
```
Start and enable nginx service
```sh
sudo systemctl enable --now nginx
```
Check status
```sh
sudo systemctl status nginx
```
### Check the active apache port on all app servers
On each app server:
```sh
sudo grep -R "^[[:space:]]*Listen" /etc/httpd/conf /etc/httpd/conf.d 2>/dev/null
```
Output
```sh
Listen 3000
```
### Configure the Nginx server
```sh
sudo vi /etc/nginx/nginx.conf
```
The http section in the main configuration:
```sh
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }
        }
    }
``` 
We have to add the below lines in the http{..} section
```sh
upstream Nautilus 
    {
    server stapp01:3000; 
    server stapp02:3000; 
    server stapp03:3000;
    }
```
And the below lines in the server {..} section
```sh
    listen 80;  
    listen [::]:80;     
    location / { proxy_pass http://Nautilus; }
```
Final http {...} Main config file will be:
```sh 
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    #include /etc/nginx/conf.d/*.conf;
    upstream Nautilus 
    {
        server stapp01:3000; 
        server stapp02:3000; 
        server stapp03:3000;
    }

    server 
    {
        listen 80;  
        listen [::]:80;     
        location / { proxy_pass http://Nautilus; }
    }
}
```
Don't forget to comment the line
`include /etc/nginx/conf.d/*.conf;`
To make nginx read from the main file as in the task.
### Validate & restart nginx
```sh
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl status nginx --no-pager
```
### Check from LBR server
```sh
curl -I http://stapp01:3000/
curl -I http://stapp02:3000/
curl -I http://stapp03:3000/
curl -I http://localhost/
```
Output
```sh
Welcome to xFusionCorp Industries!
```
