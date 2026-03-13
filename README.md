# SOC Brute Force Detection Lab

## Overview

This project simulates a brute force attack against a Windows machine using Hydra from a Kali Linux attacker machine.
The objective of the lab is to generate authentication logs and analyze them using Splunk SIEM in order to detect suspicious login activity.

This type of analysis reflects common tasks performed by SOC analysts when investigating potential brute force attacks.


## Lab Architecture

Attacker: Kali Linux
Target: Windows 10/11 with OpenSSH Server enabled
SIEM: Splunk

Attack Flow:

Kali Linux → Hydra brute force → Windows SSH service → Security logs generated → Logs analyzed in Splunk


## Tools Used

* Hydra (brute force attack simulation)
* Nmap (network scanning)
* Splunk (SIEM log analysis)
* Windows Event Viewer


## Attack Simulation

The attack was performed using Hydra from Kali Linux against the SSH service on the Windows machine.

Example command used in the lab:

hydra -l testuser -P password.txt ssh://TARGET_IP

The attack generates multiple authentication attempts in a short period of time.


## Log Generation

The attack generates Windows Security Events such as:

4625 – Failed login attempt
4624 – Successful login

Multiple Event ID 4625 entries indicate repeated authentication failures, which is a common indicator of brute force activity.

When the correct password is found, Event ID 4624 appears, indicating a successful authentication.



## Splunk Log Analysis

The Windows Security logs were analyzed using Splunk.

Example query used to identify failed login attempts:

index=* EventCode=4625

Example query for successful logins:

index=* EventCode=4624

Example query to detect potential brute force behavior:

index=* EventCode=4625
| stats count by Account_Name, Source_Network_Address

This allows SOC analysts to identify accounts targeted by multiple failed login attempts.


## Detection Logic

Possible brute force indicators include:

* Multiple failed login attempts within a short timeframe
* Repeated attempts against the same account
* Failed login attempts followed by a successful authentication

These indicators are commonly monitored by Security Operations Centers (SOC).


## Screenshots

Example screenshots to include in this repository:

* Nmap scan showing the SSH port open
* Hydra brute force attack execution
* Windows Event Viewer showing Event ID 4625
* Windows Event Viewer showing Event ID 4624
* Splunk dashboard displaying authentication logs


## Skills Demonstrated

* Brute force attack simulation
* Security log analysis
* SIEM investigation
* Windows Security Event monitoring
* Basic threat detection techniques


## Disclaimer

This lab was conducted in a controlled virtual environment for educational purposes only.

The attack was performed exclusively within a personal lab environment and did not target any real systems or networks.
