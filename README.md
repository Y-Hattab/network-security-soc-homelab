# \# Network Security \& SOC Homelab

# 

# Hands-on cybersecurity homelab focused on network segmentation, firewall hardening, centralized monitoring, intrusion detection and secure remote administration.

# 

# \## Project Overview

# 

# The objective of this project was to build a realistic segmented network environment using pfSense and several Linux virtual machines.

# 

# The lab implements:

# 

# \- Network segmentation with pfSense

# \- DMZ, USERS, ADMIN and SERVERS security zones

# \- Wazuh SIEM/XDR monitoring

# \- Suricata IDS with ET Open rules

# \- Suricata alert forwarding to Wazuh

# \- WireGuard VPN

# \- Firewall hardening and least-privilege access

# \- Security testing with SSH and Nmap

# 

# \## Network Architecture

# 

# | Zone | Network | Main Host |

# |---|---|---|

# | DMZ | `10.10.10.0/24` | DMZ-WEB `10.10.10.10` |

# | USERS | `10.20.20.0/24` | USER-PC-01 `10.20.20.10` |

# | ADMIN | `10.30.30.0/24` | ADMIN-PC-01 `10.30.30.10` |

# | SERVERS | `10.40.40.0/24` | SOC-WAZUH-01 `10.40.40.10` |

# | VPN | `10.50.50.0/24` | pfSense `10.50.50.1`, Client `10.50.50.2` |

# 

# pfSense acts as the central router and firewall between all network zones.

# 

# \## Main Components

# 

# \### pfSense

# 

# pfSense provides:

# 

# \- Inter-VLAN/network routing

# \- Stateful firewalling

# \- NAT

# \- Traffic segmentation

# \- WireGuard VPN

# \- Suricata IDS integration

# 

# Firewall policies follow a least-privilege approach.

# 

# \### Wazuh

# 

# Wazuh is deployed on `SOC-WAZUH-01`.

# 

# It provides:

# 

# \- Centralized log collection

# \- Endpoint monitoring

# \- SSH authentication monitoring

# \- Security event analysis

# \- Suricata alert correlation

# 

# Two Linux endpoints were enrolled as Wazuh agents:

# 

# \- DMZ-WEB

# \- USER-PC-01

# 

# !\[Wazuh Agents](docs/screenshots/wazuh-agents.png)

# 

# \## SSH Attack Detection

# 

# Repeated invalid SSH authentication attempts were generated against the DMZ web server.

# 

# Wazuh successfully detected:

# 

# \- Failed password attempts

# \- Invalid users

# \- Source IP address

# \- MITRE ATT\&CK related techniques

# 

# !\[Wazuh SSH Detection](docs/screenshots/wazuh-ssh-detection.png)

# 

# \## Suricata IDS

# 

# Suricata was deployed on pfSense to inspect network traffic.

# 

# ET Open signatures were enabled.

# 

# An Nmap scan generated the following real IDS alert:

# 

# `ET SCAN Possible Nmap User-Agent Observed`

# 

# !\[Suricata IDS](docs/screenshots/suricata-ids.png)

# 

# \## Suricata and Wazuh Integration

# 

# Suricata events are forwarded from pfSense to Wazuh using syslog.

# 

# A custom Wazuh decoder identifies Suricata EVE JSON events.

# 

# Local Wazuh rule `100101` detects Suricata events where:

# 

# `event\_type = alert`

# 

# Detection pipeline:

# 

# `Network Traffic → Suricata → Syslog → Wazuh → SIEM Alert`

# 

# !\[Suricata Wazuh Integration](docs/screenshots/suricata-wazuh.png)

# 

# \## WireGuard VPN

# 

# WireGuard provides secure remote access to management resources.

# 

# VPN network:

# 

# `10.50.50.0/24`

# 

# Example:

# 

# \- pfSense WireGuard interface: `10.50.50.1`

# \- VPN client: `10.50.50.2`

# 

# Without the VPN, the USERS network cannot directly access ADMIN or the Wazuh management interface.

# 

# After establishing the WireGuard tunnel, explicitly authorized resources become reachable.

# 

# !\[WireGuard VPN](docs/screenshots/wireguard-vpn.png)

# 

# \## Network Segmentation

# 

# The final firewall policy separates the environment into dedicated security zones.

# 

# \### USERS

# 

# Allowed:

# 

# \- HTTP access to DMZ web server

# \- Wazuh agent communication

# \- DNS

# \- HTTP/HTTPS Internet access

# 

# Blocked:

# 

# \- Direct ADMIN access

# \- Direct SERVERS management access

# 

# !\[User to DMZ](docs/screenshots/user-to-dmz-access.png)

# 

# \### DMZ

# 

# Allowed:

# 

# \- Wazuh agent communication

# \- DNS

# \- NTP

# \- HTTP/HTTPS Internet access

# 

# Blocked:

# 

# \- ADMIN network

# \- Wazuh Dashboard

# \- Internal management interfaces

# 

# !\[DMZ Isolation](docs/screenshots/dmz-isolation.png)

# 

# \### ADMIN

# 

# ADMIN-PC is the dedicated management workstation.

# 

# It can access:

# 

# \- pfSense WebGUI

# \- Wazuh Dashboard

# \- Wazuh server through SSH

# 

# !\[Administrative Access](docs/screenshots/admin-access.png)

# 

# \### SERVERS

# 

# The SERVERS network is isolated from other internal networks by default.

# 

# Only explicitly required outbound traffic is permitted.

# 

# !\[Servers Isolation](docs/screenshots/servers-isolation.png)

# 

# \## Security Validation

# 

# Final validation included:

# 

# | Test | Result |

# |---|---|

# | USERS → DMZ HTTP | Allowed |

# | USERS → ADMIN without VPN | Blocked |

# | USERS → Wazuh Dashboard without VPN | Blocked |

# | DMZ → ADMIN | Blocked |

# | DMZ → Wazuh Dashboard | Blocked |

# | ADMIN → pfSense | Allowed |

# | ADMIN → Wazuh Dashboard | Allowed |

# | ADMIN → Wazuh SSH | Allowed |

# | SERVERS → USERS | Blocked |

# | WireGuard → ADMIN-PC SSH | Allowed |

# | WireGuard → Wazuh HTTPS | Allowed |

# | SSH attack → Wazuh alert | Detected |

# | Nmap traffic → Suricata alert | Detected |

# | Suricata alert → Wazuh | Detected |

# 

# \## Technologies

# 

# \- VMware Workstation

# \- pfSense

# \- Ubuntu Server 24.04 LTS

# \- Ubuntu Desktop

# \- Wazuh

# \- Suricata

# \- ET Open Rules

# \- WireGuard

# \- Nginx

# \- Nmap

# \- SSH

# \- Syslog

# 

# \## Skills Demonstrated

# 

# \- Network architecture

# \- Network segmentation

# \- Firewall rule design

# \- Least-privilege access control

# \- DMZ implementation

# \- Routing and NAT

# \- Linux administration

# \- SIEM deployment

# \- IDS monitoring

# \- Log analysis

# \- Security event correlation

# \- VPN deployment

# \- Network troubleshooting

# \- Security validation

# 

# \## Future Improvements

# 

# Possible future improvements:

# 

# \- Suricata IPS mode after signature tuning

# \- Dedicated vulnerability scanner

# \- Automated infrastructure deployment

# \- Additional attack scenarios

# \- Centralized metrics and monitoring

# \- Windows Active Directory security lab as a separate project

# 

# \## Disclaimer

# 

# This project was built in an isolated academic homelab for educational and defensive cybersecurity purposes.

