# Personal-SOC-Lab-T-Pot-CE-

## Quick Summary

I deployed an AWS EC2 instance running Linux and installed T-Pot CE to build a personal SOC lab. Over seven days, real-world attack telemetry was collected by intentionally exposing 10 containerized honeypots to the public internet.

SOC analysis and learning was performed through T-Pot’s web interface, which allowed me to monitor honeypot interactions and attacker behavior. Further analysis and findings are documented throughout this repository.

---
***Overall, this project provided hands-on learning experience through exporting and filtering logs to study specific attacks. I also learned how to distinguish background scan noise from legitimate threats.***
---

## Architecture Topology

![Topology](https://github.com/ShaneYeung-Cyber/Personal-SOC-Lab-T-Pot-CE-/blob/main/Cloud-Based%20Cybersecurity%20Homelab.png)

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

When an interaction occured with a honeypot, detailed logs were generated. Some of the many detailed information include source IP information, timestamps, targeted services, and any commands/payloads. These attacks were real and not simulated traffic. The screenshot below is one of the dashboad available with this project.
