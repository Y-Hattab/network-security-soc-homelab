# pfSense Firewall Rules



The firewall policy follows a least-privilege approach.



Rules are processed from top to bottom and the first matching rule is applied.



## DMZ



Allowed:



- DMZ → Wazuh server TCP `1514-1515`

- DNS TCP/UDP `53`

- NTP UDP `123`

- HTTP TCP `80`

- HTTPS TCP `443`



Blocked:



- DMZ → `INTERNAL\\_NETWORKS` using IPv4 any



The Wazuh agent exception is placed above the internal network block rule.



## USERS



Allowed:



- USER-PC → pfSense UDP `51820` for WireGuard testing

- USERS → Wazuh TCP `1514-1515`

- USERS → DMZ-WEB TCP `80`

- DNS TCP/UDP `53`

- HTTP TCP `80`

- HTTPS TCP `443`



Blocked:



- USERS → `INTERNAL\\_NETWORKS` using IPv4 any



This prevents direct access to ADMIN and SERVERS while keeping explicitly authorized flows available.



## ADMIN



Allowed:



- ADMIN → pfSense TCP `443`

- ADMIN → Wazuh TCP `443`

- ADMIN → Wazuh TCP `22`

- DNS TCP/UDP `53`

- NTP UDP `123`

- ICMP for diagnostics

- HTTP/HTTPS to destinations outside `INTERNAL\\_NETWORKS`



ADMIN is the dedicated management zone.



## SERVERS



Allowed:



- DNS TCP/UDP `53`

- NTP UDP `123`

- HTTP TCP `80`

- HTTPS TCP `443`

- ICMP for diagnostics



Blocked:



- SERVERS → `INTERNAL\\_NETWORKS` using IPv4 any



The internal block is placed before generic outbound Internet rules.



## WireGuard



Allowed:



- VPN → pfSense ICMP

- VPN → ADMIN ICMP

- VPN → SERVERS ICMP

- VPN → Wazuh TCP `443`

- VPN → Wazuh TCP `1514-1515`

- VPN → ADMIN-PC TCP `22`



The VPN only provides access to explicitly authorized management services.



## Hardening Decisions



During the final hardening phase:



- pfSense WebGUI access from DMZ was removed

- direct Wazuh management access from USERS was removed

- broad allow-any rules were removed

- ADMIN became the primary local management zone

- WireGuard became the controlled remote-access path





