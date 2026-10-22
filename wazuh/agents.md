\# Wazuh Agents and Detection



Wazuh is deployed on `SOC-WAZUH-01` with the following address:



`10.40.40.10`



The server hosts:



\- Wazuh Manager

\- Wazuh Indexer

\- Wazuh Dashboard



\## Agents



Two Linux endpoints are monitored:



| Agent | IP | Role |

|---|---|---|

| DMZ-WEB | `10.10.10.10` | DMZ web server |

| USER-PC-01 | `10.20.20.10` / VPN `10.50.50.2` | User workstation |



Wazuh agent communication uses:



\- TCP `1514` for event communication

\- TCP `1515` for enrollment



\## SSH Detection Scenario



Repeated invalid SSH login attempts were generated from USER-PC-01 against DMZ-WEB.



Example source:



`10.20.20.10`



Target:



`10.10.10.10`



Wazuh detected:



\- failed password attempts

\- invalid usernames

\- source IP address

\- target system

\- SSH authentication activity



One of the observed Wazuh rules was:



\- Rule ID: `5710`

\- Description: `sshd: Attempt to login using a non-existent user`



\## Suricata Integration



Wazuh also receives Suricata IDS events from pfSense.



Custom rule `100101` identifies Suricata alerts and integrates network IDS events into the SIEM workflow.



\## Validation



Final validation confirmed both monitored agents as active in the Wazuh Dashboard.



