# Step 07 — Linux Networking

---

## Overview

Networking is one of the most important topics in Linux system administration and DevOps. Every Linux server communicates through network interfaces, IP addresses, routing tables, DNS servers, sockets, and network services.

In this chapter, we learned how Linux networking works, how to inspect network configuration, verify connectivity, troubleshoot DNS resolution, inspect listening ports, and interact with remote web services.

This knowledge forms the foundation for SSH, Docker networking, Kubernetes networking, cloud infrastructure, monitoring, and production server administration.

---

## Learning Objectives

After completing this chapter, you will be able to:

- Understand Linux networking fundamentals
- Identify hostnames and IP addresses
- Inspect network interfaces
- Read routing tables
- Understand gateways and routing
- Understand DNS resolution
- Test network connectivity
- Inspect listening ports and sockets
- Use modern networking utilities
- Perform basic DNS troubleshooting
- Download web resources using command-line tools
- Prepare for SSH, Docker, Kubernetes, and Cloud Networking

---

# Networking Workflow

```text
Application
      │
      ▼
Hostname
      │
      ▼
DNS Resolution
      │
      ▼
IP Address
      │
      ▼
Routing Table
      │
      ▼
Gateway
      │
      ▼
Network Interface
      │
      ▼
Remote Server
```

---

# Topics Covered

1. Hostname
2. IP Address
3. Network Interfaces
4. Routing
5. DNS
6. Network Testing
7. Socket Inspection
8. HTTP Client Tools
9. DNS Troubleshooting
10. Remote Networking Tools

---

# Command List

| Command | Purpose |
|----------|----------|
| hostname | Show system hostname |
| hostname -I | Show IP addresses |
| hostnamectl | Show system information |
| ip addr | Show IP configuration |
| ip -br addr | Show brief IP information |
| ip link | Show network interfaces |
| ip -br link | Show brief interface information |
| ip addr show | Show interface addresses |
| ip addr show enp0s3 | Show a specific interface |
| ip route | Show routing table |
| route -n | Show legacy routing table |
| cat /etc/hosts | Show local hostname mappings |
| cat /etc/resolv.conf | Show resolver configuration |
| resolvectl status | Show DNS resolver information |
| ping | Test connectivity |
| ss | Show socket statistics |
| curl | HTTP client |
| wget | Download files |
| host | Quick DNS lookup |
| nslookup | DNS query tool |
| dig | Advanced DNS lookup |
| which | Locate executable |
| ssh -V | Show SSH version |
| scp | Secure file copy |

---

# Command Categories

| Category | Commands |
|-----------|----------|
| Host Information | hostname, hostnamectl |
| IP Configuration | ip addr, ip link |
| Routing | ip route, route |
| DNS | hosts, resolv.conf, resolvectl |
| Connectivity | ping |
| Socket Inspection | ss |
| HTTP Tools | curl, wget |
| DNS Debugging | host, nslookup, dig |
| Remote Access | ssh, scp |

---

# Command Variations

## hostname

```bash
hostname
hostname -I
hostnamectl
```

| Option | Meaning |
|----------|----------|
| -I | Show IP addresses |

---

## ip

```bash
ip addr
ip -br addr
ip link
ip -br link
ip addr show
ip addr show enp0s3
ip route
```

| Option | Meaning |
|----------|----------|
| addr | Address information |
| link | Network interfaces |
| route | Routing table |
| show | Display object |
| -br | Brief output |

---

## Network Information Commands Comparison

| Command | Shows | Best Use |
|----------|---------|----------|
| hostname | Computer name | Quick hostname |
| hostnamectl | Full system information | System inspection |
| ip addr | Complete IP details | Networking |
| ip -br addr | Short summary | Daily administration |
| ip link | Interface information | Network adapters |
| ip route | Routing table | Routing troubleshooting |

---

## Important Networking Terms

| Term | Meaning |
|------|----------|
| Hostname | Computer name |
| Interface | Network adapter |
| IPv4 | 32-bit IP address |
| IPv6 | 128-bit IP address |
| Loopback | Local communication interface |
| Gateway | Router to other networks |
| Route | Path taken by packets |
| Routing Table | Collection of network routes |
| DNS | Domain Name System |
| Resolver | DNS client |
| Nameserver | DNS server |
| Socket | Communication endpoint |
| Port | Service communication endpoint |
| MAC Address | Hardware address |
| MTU | Maximum Transmission Unit |

# Routing

Linux uses a routing table to determine where packets should be sent. Every outgoing packet is matched against the routing table. If no specific route matches, Linux uses the default gateway.

---

## Routing Workflow

```text
Application
      │
      ▼
Destination IP
      │
      ▼
Routing Table
      │
      ├── Same Network
      │         │
      │         ▼
      │   Direct Communication
      │
      ▼
Different Network
      │
      ▼
Default Gateway
      │
      ▼
Remote Network
```

---

## Commands

```bash
ip route

route -n
```

---

## Command Comparison

| Command | Purpose | Recommendation |
|----------|----------|----------------|
| ip route | Modern routing table | Recommended |
| route -n | Legacy routing table | Compatibility |

---

## Important Routing Terms

| Term | Meaning |
|------|----------|
| Route | Path used by packets |
| Gateway | Router connecting different networks |
| Default Route | Fallback route |
| Destination | Target network |
| Metric | Route priority (lower value = higher priority) |

---

# DNS (Domain Name System)

DNS converts human-readable domain names into IP addresses.

Example:

```text
google.com
      │
      ▼
142.251.xxx.xxx
```

---

## DNS Resolution Workflow

```text
Application
      │
      ▼
Domain Name
      │
      ▼
/etc/hosts
      │
      ▼
systemd-resolved
(127.0.0.53)
      │
      ▼
DNS Server
      │
      ▼
IP Address
```

---

## Commands

```bash
cat /etc/hosts

cat /etc/resolv.conf

resolvectl status
```

---

## Command Comparison

| Command | Purpose |
|----------|----------|
| cat /etc/hosts | Local hostname mappings |
| cat /etc/resolv.conf | DNS resolver configuration |
| resolvectl status | DNS resolver status |

---

## Important DNS Files

| File | Purpose |
|------|----------|
| /etc/hosts | Local hostname database |
| /etc/resolv.conf | Resolver configuration |

---

## DNS Terminology

| Term | Meaning |
|------|----------|
| DNS | Domain Name System |
| Resolver | DNS client |
| Nameserver | DNS server |
| Domain | Human-readable hostname |
| IP Address | Machine-readable address |

---

# Network Connectivity Testing

Connectivity testing verifies whether a host is reachable.

---

## Workflow

```text
Local Machine
      │
      ▼
ICMP Echo Request
      │
      ▼
Remote Host
      │
      ▼
ICMP Echo Reply
```

---

## Commands

```bash
ping -c 4 127.0.0.1

ping -c 4 localhost

ping -c 4 8.8.8.8

ping -c 4 google.com
```

---

## Command Comparison

| Command | Purpose |
|----------|----------|
| ping 127.0.0.1 | Test TCP/IP stack |
| ping localhost | Test hostname resolution |
| ping 8.8.8.8 | Test Internet connectivity |
| ping google.com | Test Internet + DNS |

---

## Ping Option

| Option | Meaning |
|----------|----------|
| -c | Number of packets to send |

---

## Ping Output Explanation

| Field | Meaning |
|------|----------|
| icmp_seq | Packet sequence number |
| ttl | Time To Live |
| time | Response time |
| packet loss | Lost packets |
| rtt | Round Trip Time |
| min | Minimum latency |
| avg | Average latency |
| max | Maximum latency |
| mdev | Latency variation |

---

# Socket Statistics

Linux sockets represent communication endpoints used by network services.

The `ss` command is the modern replacement for `netstat`.

---

## Socket Workflow

```text
Application
      │
      ▼
Socket
      │
      ▼
Port
      │
      ▼
Listening Service
      │
      ▼
Client Connection
```

---

## Commands

```bash
ss -tuln

ss -tunap

ss -ltn
```

---

## ss Options

| Option | Meaning |
|----------|----------|
| -t | TCP sockets |
| -u | UDP sockets |
| -l | Listening sockets |
| -a | All sockets |
| -n | Numeric output |
| -p | Show process |

---

## Command Comparison

| Command | Purpose |
|----------|----------|
| ss -tuln | Listening TCP & UDP sockets |
| ss -tunap | All sockets with process information |
| ss -ltn | Listening TCP sockets only |

---

## Common Socket States

| State | Meaning |
|--------|----------|
| LISTEN | Waiting for incoming connection |
| ESTAB | Established connection |
| CLOSE-WAIT | Waiting for application to close |
| TIME-WAIT | Waiting before socket closes |

---

## Common Ports

| Port | Service |
|------|----------|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 631 | CUPS Printing |

---

# HTTP Client Tools

Linux provides command-line tools for communicating with web servers.

---

## curl vs wget

| Tool | Primary Purpose |
|------|------------------|
| curl | Send HTTP requests |
| wget | Download files |

---

## HTTP Workflow

```text
Client
     │
     ▼
HTTP Request
     │
     ▼
Web Server
     │
     ▼
HTTP Response
```

---

## Commands

```bash
curl https://example.com

curl -I https://example.com

wget https://example.com

ls -l index.html
```

---

## Command Comparison

| Command | Purpose |
|----------|----------|
| curl URL | Display response body |
| curl -I URL | Display HTTP headers only |
| wget URL | Download resource |
| ls -l | Verify downloaded file |

---

## curl Options

| Option | Meaning |
|----------|----------|
| -I | Fetch HTTP headers only (HEAD request) |

---

## HTTP Status Codes

| Code | Meaning |
|------|----------|
| 200 | OK |
| 301 | Moved Permanently |
| 302 | Redirect |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

# DNS Troubleshooting

Linux provides multiple utilities for troubleshooting DNS resolution. Although they perform similar tasks, each tool presents information differently.

---

## DNS Lookup Workflow

```text
Domain Name
      │
      ▼
DNS Query
      │
      ▼
DNS Resolver
      │
      ▼
DNS Server
      │
      ▼
Response
      │
      ▼
IPv4 / IPv6 Address
```

---

## Commands

```bash
host google.com

nslookup google.com

dig google.com
```

---

## Command Comparison

| Command | Output | Best Use |
|----------|---------|----------|
| host | Short | Quick lookup |
| nslookup | Medium | Verify DNS server |
| dig | Detailed | DNS troubleshooting |

---

## Common DNS Records

| Record | Purpose |
|----------|----------|
| A | IPv4 Address |
| AAAA | IPv6 Address |
| MX | Mail Server |
| NS | Name Server |
| CNAME | Alias Record |
| TXT | Text Record |

---

## Important dig Sections

| Section | Purpose |
|----------|----------|
| HEADER | Query status |
| QUESTION SECTION | Requested record |
| ANSWER SECTION | DNS answer |
| AUTHORITY SECTION | Authoritative servers |
| ADDITIONAL SECTION | Extra information |

---

## Common Query Status

| Status | Meaning |
|----------|----------|
| NOERROR | Query successful |
| NXDOMAIN | Domain not found |
| SERVFAIL | Server failure |

---

# Remote Access Tools

Linux servers are commonly managed remotely.

---

## Commands

```bash
which ssh

ssh -V

which scp

scp

which curl

which wget
```

---

## Command Comparison

| Command | Purpose |
|----------|----------|
| ssh | Remote login |
| scp | Secure file copy |
| curl | HTTP client |
| wget | File downloader |
| which | Locate executable |

---

## SSH vs SCP

| Tool | Purpose |
|------|----------|
| ssh | Remote terminal access |
| scp | Secure file transfer |

---

# Output Navigation

Many Linux commands use the **less** pager to display long output.

Examples:

- man
- systemctl
- journalctl
- dmesg
- git log

---

## Pager Navigation

| Key | Action |
|------|---------|
| ↑ ↓ | Scroll |
| Enter | Next line |
| Space | Next page |
| b | Previous page |
| g | First line |
| Shift + G | Last line |
| /word | Search |
| n | Next search result |
| q | Quit |

---

## Process Control vs Pager Navigation

| Situation | Key |
|-----------|------|
| Exit pager (`less`, `man`, `journalctl`, `systemctl`) | q |
| Stop running process | Ctrl + C |
| Suspend running process | Ctrl + Z |

---

# Command Summary

| Category | Commands |
|-----------|----------|
| Host Information | hostname, hostnamectl |
| Network Interface | ip addr, ip link |
| Routing | ip route, route |
| DNS Configuration | hosts, resolv.conf, resolvectl |
| DNS Troubleshooting | host, nslookup, dig |
| Connectivity | ping |
| Socket Inspection | ss |
| HTTP Tools | curl, wget |
| Remote Access | ssh, scp |
| Executable Lookup | which |

---

# Linux Networking Cheat Sheet

| Task | Command |
|------|----------|
| Show hostname | hostname |
| Show system information | hostnamectl |
| Show IP addresses | hostname -I |
| Show interfaces | ip link |
| Show interface addresses | ip addr |
| Show brief interface information | ip -br addr |
| Show routing table | ip route |
| Show DNS configuration | cat /etc/resolv.conf |
| Show hosts file | cat /etc/hosts |
| Show DNS resolver | resolvectl status |
| Test localhost | ping 127.0.0.1 |
| Test Internet | ping 8.8.8.8 |
| Test DNS | ping google.com |
| Show listening ports | ss -tuln |
| Show all sockets | ss -tunap |
| Show listening TCP sockets | ss -ltn |
| Send HTTP request | curl URL |
| Show HTTP headers | curl -I URL |
| Download file | wget URL |
| DNS lookup | host domain |
| DNS query | nslookup domain |
| Advanced DNS query | dig domain |
| Locate executable | which command |
| SSH version | ssh -V |

---

# Best Practices

- Prefer the modern `ip` command over legacy networking tools whenever possible.
- Use `ss` instead of `netstat`.
- Use `curl` for API testing and HTTP requests.
- Use `wget` for downloading files.
- Always verify DNS before troubleshooting network connectivity.
- Use `hostnamectl` instead of manually checking multiple files.
- Read routing tables before modifying network configuration.
- Avoid editing `/etc/resolv.conf` directly when it is managed by `systemd-resolved`.
- Use `ip -br` commands for quick diagnostics.
- Learn both modern (`ip`) and legacy (`route`) commands for compatibility with older systems.

---

# Key Takeaways

After completing this chapter, you can now:

- Identify Linux host information
- Understand IPv4 and IPv6 addressing
- Inspect network interfaces
- Read routing tables
- Understand gateways and routing
- Understand DNS resolution
- Test network connectivity
- Troubleshoot DNS
- Inspect sockets and listening ports
- Use HTTP command-line tools
- Download resources from the Internet
- Verify remote access utilities
- Navigate long terminal output efficiently

These skills provide the networking foundation required for SSH administration, Docker networking, Kubernetes networking, cloud infrastructure, CI/CD pipelines, monitoring, and production Linux server management.

---


