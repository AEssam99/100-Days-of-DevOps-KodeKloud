# Day 14: Linux Process Troubleshooting
## Task
The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. 
running on the systems. 
One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.

Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. 
They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. 
Just make sure the service is up and running. Also, make sure Apache is running on port 8083 on all app servers.
## Solution
### Check Apache status
```sh
sudo systemctl status httpd
```
```sh
httpd.service - The Apache HTTP Server 
Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled) 
Active: failed (Result: exit-code) since Wed 2026-02-25 08:38:45 UTC; 2min 39s ago 
Docs: man:httpd(8) 
man:apachectl(8) 
Process: 820 ExecStop=/bin/kill -WINCH ${MAINPID} 
(code=exited, status=1/FAILURE) 
Process: 819 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE) 
Main PID: 819 (code=exited, status=1/FAILURE)
```
### Check Journalctl logs
```sh
sudo journalctl -xeu httpd
```
```sh
-- Logs begin at Wed 2026-02-25 08:38:29 UTC, end at Wed 2026-02-25 08:43:08 UTC. --
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com systemd[1]: Starting The Apache HTTP Server...
-- Subject: Unit httpd.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit httpd.service has begun starting up.
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com httpd[819]: AH00558: httpd: Could not reliably determine the server's fully q
ualified domain name, using stapp01.stratos.xfusioncorp.com. Set the 'ServerName' directive globally to suppress this message
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com httpd[819]: (98)Address already in use: AH00072: make_sock: could not bind to
 address 0.0.0.0:8083
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com httpd[819]: no listening sockets available, shutting down
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com httpd[819]: AH00015: Unable to open logs
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: main process exited, code=exited, status=1/F
AILURE
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com kill[820]: kill: cannot find process ""
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: control process exited, code=exited status=1

Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com systemd[1]: Failed to start The Apache HTTP Server.
-- Subject: Unit httpd.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit httpd.service has failed.
-- 
-- The result is failed.
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com systemd[1]: Unit httpd.service entered failed state.
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service failed.
```
The line following line indicated there is an issue with port 8083
```sh
Feb 25 08:38:45 stapp01.stratos.xfusioncorp.com httpd[819]: (98)Address already in use: AH00072: make_sock: could not bind to
 address 0.0.0.0:8083
```
### Check port 8083 status
```sh
sudo netstat -tulnp | grep 8083
```
`output`
```sh
tcp        0      0 127.0.0.1:8083          0.0.0.0:*               LISTEN      794/sendmail: accept
```
Again it's the sendmail service
### Disbale and stop sendmail service
```sh
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```
### Restart httpd service
```sh 
sudo systemctl restart httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```
`Output`
```sh 
httpd.service - The Apache HTTP Server 
Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled) 
Active: active (running) since Wed 2026-02-25 09:12:43 UTC; 21s ago
``` 
It works now.
