# IP Addressing Plan



This lab is divided into multiple security zones connected through pfSense.



| Zone | Network | Gateway | Main Host |

|---|---|---|---|

| DMZ | `10.10.10.0/24` | `10.10.10.1` | DMZ-WEB `10.10.10.10` |

| USERS | `10.20.20.0/24` | `10.20.20.1` | USER-PC-01 `10.20.20.10` |

| ADMIN | `10.30.30.0/24` | `10.30.30.1` | ADMIN-PC-01 `10.30.30.10` |

| SERVERS | `10.40.40.0/24` | `10.40.40.1` | SOC-WAZUH-01 `10.40.40.10` |

| VPN | `10.50.50.0/24` | `10.50.50.1` | WireGuard client `10.50.50.2` |

| WAN | VMware NAT | DHCP | pfSense WAN |



## VMware Networks



- VMnet8: WAN / NAT

- VMnet2: DMZ

- VMnet3: USERS

- VMnet4: ADMIN

- VMnet5: SERVERS



## pfSense Interfaces



- WAN → VMware NAT

- DMZ → `10.10.10.1/24`

- USERS → `10.20.20.1/24`

- ADMIN → `10.30.30.1/24`

- SERVERS → `10.40.40.1/24`

- WireGuard → `10.50.50.1/24`



## INTERNAL_NETWORKS Alias



The pfSense alias `INTERNAL_NETWORKS` contains:



- `10.10.10.0/24`

- `10.20.20.0/24`

- `10.30.30.0/24`

- `10.40.40.0/24`

- `10.50.50.0/24`



This alias is used to simplify internal segmentation rules.





