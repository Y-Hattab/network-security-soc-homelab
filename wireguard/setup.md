# WireGuard VPN Setup



WireGuard is configured on pfSense to provide secure remote access to management resources.



## VPN Addressing



VPN network:



`10.50.50.0/24`



pfSense WireGuard interface:



`10.50.50.1/24`



VPN client:



`10.50.50.2/32`



## Client Routing



The WireGuard client routes the following networks through the VPN:



- `10.30.30.0/24`

- `10.40.40.0/24`

- `10.50.50.0/24`



This allows access to ADMIN, SERVERS and the VPN network through the encrypted tunnel.



## Peer Configuration



pfSense associates the VPN client public key with:



`10.50.50.2/32`



The private keys are never stored in this repository.



## Firewall Policy



The WireGuard firewall rules authorize only required traffic.



Examples:



- VPN → ADMIN-PC TCP `22`

- VPN → Wazuh TCP `443`

- VPN → Wazuh TCP `1514-1515`

- ICMP for diagnostic testing



## Validation



The tunnel was validated using:



`wg`



A successful configuration displayed:



- a recent handshake

- transferred and received traffic



Remote SSH access to ADMIN-PC was also successfully tested through WireGuard.



## Security Model



Without the VPN:



- USERS cannot directly access ADMIN

- USERS cannot directly access Wazuh management services



With the VPN:



- explicitly authorized ADMIN and Wazuh services become accessible



This provides a controlled remote administration path.



