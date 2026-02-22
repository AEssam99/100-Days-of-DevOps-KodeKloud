# Day 12: Linux Network Services
## Task
Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 6100 (which is the Apache port). 
The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command curl http://stapp01:6100 command from jump host.

Note: Please do not try to alter the existing index.html code, as it will lead to task failure.

## Solution

### Trying to connect stapp01 via ssh from jump host
### Checking httpd Service status
### Checking who is using port 6100
### Stop sendmail (Safe Fix) 
### Verifying 
### Check firewall
### Check from jump host
