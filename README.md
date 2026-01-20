# Personal-SOC-Lab-T-Pot-CE-

## Quick Summary

I deployed an AWS EC2 instance running Linux and installed T-Pot CE to build a personal SOC lab. Over seven days, real-world attack telemetry was collected by intentionally exposing 10 containerized honeypots to the public internet.

SOC analysis and learning was performed through T-Pot’s web interface, which allowed me to monitor honeypot interactions and attacker behavior. Further analysis and findings are documented throughout this repository.

---
***Overall, this project provided hands-on learning experience through exporting and filtering logs to study specific attacks. I also learned how to distinguish background scan noise from legitimate threats.***
---

## Architecture Topology

![Topology](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Cloud-Based%20Cybersecurity%20Homelab.png?raw=true)
This topology represents a public-facing AWS environment specifically designed to collect real-world attack telemetry using containerized honeypots for sole-purpose of SOC analysis and learning.

## Environment Overview

AWS EC2 Instance : t2.xlarge (4 vcpu, 16 Gib Memory)

Region : Us-East-1 (N. Virginia)

Storage : 300 GB Amazon EBS volume

Linux OS : Ubuntu Server 24.04 LTS (64-bit (x86)

Exposure duration : 7 days

Number of honeypots enabled : 10

**There has been up to 4000 documented attack events per hour so 16 GiB memory was necessary for log ingestion, indexing and visualization through Elastic Stack (Elasticsearch and Kibana)**

## Data Collection Methodology

This lab collected telemetry by intentionally exposing a cloud-hosted environment to public internet. Security group rules allowed inbound traffic to selected ports which in turn enabling real-world reconnaissance, scanning activity and exploitation attempts. Each of the honeypots were emulating a specific service or protocol.

![Firewall Security Rules](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Firewall%20rules.png?raw=true)

Screenshot above shows port 1-64000 were opened to capture attacker traffic.  Port 64295 was reserved for Admin SSH access and Port 64297 was for T-Pot web interface.

![T-Pot Attack Map Dashboard](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Attack%20Map.png?raw=true)

When an interaction occured with a honeypot, detailed logs were generated. Some of the many detailed information include source IP information, timestamps, targeted services, and any commands/payloads. These attacks were real and not simulated traffic.

The dots on the map represent all the countries that have attacked my server. The "Top Countries" tab highlights the protocols interacted by country , attack volume, and unique source IP counts per country. (Data displayed in this interface is refreshed every 24 hours due to caching)

## Honeypot Activity Summary 

Inbound traffic was not evenly distributed across deployed honeypots. Common services like management and remote-access protocols generated the highest volume of interactions, while less commonly targeted services saw miniscule activity.

---
**Cowrie Honeypot** recorded all activity on SSH(22) and Telnet(23).  When a connection was sucessfully established, the remote commands they inputted were logged before execution.  This provided information about the attacker's intent.
---

As you can see in the screenshot below, these were the top 10 commands inputted by the attackers. It appears to be commands focused on shell validation and system identifcation.

"cat /proc/uptime 2>/dev/null | cut -d. -f1" - Used to check host uptime during initial environment validation

![Cowrie Commands](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Cowrie%20Top%2010%20COmmands.png?raw=true)

---
**Honeytrap** was used to monitor network interaction across broad range of ports. 
---

Unlike service-specific honeypot like the one you seen above, Honeytrap does not emulate a single protocol or two. Instead it captures connection attempts and raw data sent during generic scanning and early-stage reconnaissance. Activity observed by honeytrap was typically short-lived. Most interactions did not progress beyond initial contact, which indicated automated scanning. This pattern is consistent with automated scanning.

The screenshot below shows unconventional ports being repeatedly scanned by IPs from United States and France. Timestamp show they were happening every 10-20 seconds. Over the 7 day period, these ports were probed for more than ***140,000 times***.

![HoneyTrap Traffic](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/HoneyTrap.png?raw=true)

---
**Dionaea** was configured to listen on multiple commonly exploited service ports. This includes SMB(445), NetBIOS(139), MS RPC(135), FTP (21), HTTP(80), HTTPS(443), MS SQL Server(1433), and MySQL(3306). These services are frequent targets of automated exploitation campaigns.
---

![Dionaea Attack Map](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Dionaea2.png?raw=true)

![Dionaea Pie chart](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Dionaea.png?raw=true)

Screenshots above shows all the countries that have targeted these common ports. **SMB(445)** accounted for more than 85% of the observed traffic. These ports were targeted more than **56,000 times**.  

***Dionaea's*** activity represented exploit intent which complimented HoneyTrap's scan-level visibility and Cowrie's post-access interaction data.


![Honeypot Total Attacks](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Honeypot%20Summary.png?raw=true)
![Honeypot Chart](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Images/Honeypot%20Summary2.png?raw=true)

Screenshot above shows summarizes total interactions recorded across all deployed honeypots. Over ***259,000*** were captured over 7 days with Honeytrap sitting at #1. 

---
This distribution mirrors real-world SOC conditions and demonstrates practical SOC triage by seperating background noise from actionable security events.
---
## Log Analysis and Filtering






