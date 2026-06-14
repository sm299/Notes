# Domain Name System

User -> Internet Service Provider -> Domain Name Service Resolver ->Root server<br>
## Root Servers
![alt text](image.png)

## Flow
![alt text](image-1.png)
```
User
  ↓
Web Browser
  ↓
Domain Name System Resolver
  (Internet Service Provider Resolver, Google Public Domain Name System,
   Cloudflare Domain Name System, etc.)
  ↓
Domain Name System Root Name Server
  ↓
Top-Level Domain Name Server
  (.com, .org, .net, .in, etc.)
  ↓
Authoritative Domain Name System Name Server
  ↓
Internet Protocol Address Returned
  ↓
Web Server or Load Balancer
  ↓
Hypertext Transfer Protocol Secure Request
  ↓
Website Application
  ↓
Website Response Sent Back to User
```
