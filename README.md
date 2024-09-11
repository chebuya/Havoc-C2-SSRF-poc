# CVE-2024-41570: Havoc-C2-SSRF-poc
This vulnerability is exploited by spoofing a demon agent registration and checkins to open a TCP socket on the teamserver and read/write data from it. This allows attackers to leak origin IPs of teamservers and much more.

Full analysis: https://blog.chebuya.com/posts/server-side-request-forgery-on-havoc-c2/

https://github.com/user-attachments/assets/9ab99915-8a8c-4bbd-b762-00280a23703c

To hotpatch your teamserver:

1) Navigate to the Havoc directory
2) Run the command
```bash
sed -i '/case COMMAND_SOCKET:/,/return true/d' teamserver/pkg/agent/agent.go
```
3) Rebuild the teamserver
```bash
make ts-build
```

```
usage: exploit.py [-h] -t TARGET -i IP -p PORT [-A USER_AGENT] [-H HOSTNAME] [-u USERNAME] [-d DOMAIN_NAME]
                  [-n PROCESS_NAME] [-ip INTERNAL_IP]

options:
  -h, --help            show this help message and exit
  -t TARGET, --target TARGET
                        The listener target in URL format
  -i IP, --ip IP        The IP to open the socket with
  -p PORT, --port PORT  The port to open the socket with
  -A USER_AGENT, --user-agent USER_AGENT
                        The User Agent for the spoofed agent
  -H HOSTNAME, --hostname HOSTNAME
                        The hostname for the spoofed agent
  -u USERNAME, --username USERNAME
                        The username for the spoofed agent
  -d DOMAIN_NAME, --domain-name DOMAIN_NAME
                        The domain name for the spoofed agent
  -n PROCESS_NAME, --process-name PROCESS_NAME
                        The process name for the spoofed agent
  -ip INTERNAL_IP, --internal-ip INTERNAL_IP
                        The internal ip for the spoofed agent
```
