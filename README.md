# Deep Dive into Port 1524 and System Takeover

## Overview

This project demonstrates how a single exposed network service can significantly impact the security posture of a system when improperly configured. The assessment focuses on TCP Port 1524 exposed on the Metasploitable 2 virtual machine and examines how information gathered during service enumeration can lead to direct system access within a controlled laboratory environment.

The project begins by identifying and validating the service operating on Port 1524 before progressing into system reconnaissance, credential discovery, and password analysis. Following successful access validation, internal reconnaissance techniques are used to identify operating-system characteristics, running services, active processes, and credential-related artifacts stored on the target system.

The assessment concludes with password-hash extraction and credential verification using John the Ripper, demonstrating how exposed services and weak authentication practices can contribute to wider system compromise when combined throughout an attack lifecycle.

All activities were conducted within an isolated VirtualBox Host-Only laboratory environment using intentionally vulnerable systems for educational and cybersecurity research purposes.

## Project Objectives

- Analyze the service exposed on TCP Port 1524.
- Validate service accessibility through manual interaction.
- Examine the security implications of exposed shell services.
- Demonstrate controlled system access within an isolated laboratory environment.
- Perform internal reconnaissance following successful access.
- Enumerate active processes and network services.
- Discover credential-related artifacts within the target system.
- Extract password hashes from authentication repositories.
- Analyze password hashes using John the Ripper.
- Understand the relationship between exposed services and credential compromise.
- Document security observations and defensive considerations throughout the assessment.

## Scope and Ethical Statement

All activities demonstrated in this project were performed within an isolated laboratory environment consisting of a Kali Linux attacker machine and a Metasploitable 2 target machine.

The environment was configured using a VirtualBox Host-Only Adapter network to ensure all assessment activities remained confined to an authorized testing environment.

The purpose of this project is educational and focuses on understanding security weaknesses, attacker methodologies, authentication risks, and defensive security principles within intentionally vulnerable systems.

## Lab Environment

Attacker System
- Operating System: Kali Linux
- Role: Assessment Machine
- Platform: Oracle VirtualBox

Target System
- Operating System: Metasploitable 2
- Role: Vulnerable Target
- Platform: Oracle VirtualBox

Network Configuration
- Network Type: Host-Only Adapter
- Virtualization Platform: Oracle VirtualBox

## Purpose:

- Isolated penetration-testing environment
- Controlled communication between hosts
- No Internet exposure
- Safe cybersecurity research environment

## Tools and Technologies

- Kali Linux
- Metasploitable 2
- Oracle VirtualBox
- Nmap
- Netcat (nc)
- John the Ripper
- Linux Networking Utilities
- Linux Enumeration Commands
- Password Analysis Techniques

## Skills Demonstrated

- Service Enumeration
- Port Analysis
- Service Validation
- Internal System Reconnaissance
- Process Enumeration
- Network Service Discovery
- Credential Discovery
- Password Hash Analysis
- Security Assessment Methodology
- Technical Documentation

# Phase 1 – The Anatomy of the Entry Point (Port 1524)

## Overview

The objective of this phase was to investigate TCP Port 1524 and determine the nature of the service exposed by the target system. Service identification and manual validation were performed to understand the functionality, accessibility, and security implications of the discovered service.

This phase represents the initial transition from reconnaissance to access validation. By examining the service associated with Port 1524, the assessment aimed to determine whether the exposed service provided a viable entry point into the target environment.

Sub Step 1: Service Identification and Banner Enumeration

Command Executed

Identifying the service running on TCP Port 1524...
- sudo nmap -p 1524 -sV 192.168.56.20

Results

![alt text](1.1_target_verification_port_1524.png)

Observation

Service enumeration confirmed that TCP Port 1524 was open and accessible on the target system.

The scan identified:

1524/tcp open bindshell

and further identified the service as:

Metasploitable root shell

The host was confirmed as reachable, and service fingerprinting successfully determined the nature of the application exposed on the port.

Analysis

The service operating on Port 1524 differed significantly from conventional network services such as HTTP, FTP, or SSH. Rather than providing access to a specific application, the identified service appeared to expose direct operating-system interaction capabilities.

This discovery immediately elevated the importance of Port 1524 within the assessment because exposed shell services frequently represent high-risk attack surfaces. Unlike traditional services that restrict users to predefined functionality, shell services may provide direct command execution capabilities on the target host.

The findings demonstrate the importance of service-enumeration activities during reconnaissance because accurate service identification allows analysts to prioritize targets based on potential security impact.

Sub Step 2: Manual Service Validation and Privilege Verification

Command Executed

Connecting directly to the discovered service...
- nc 192.168.56.20 1524

Verification Commands

- whoami
- id

Results

![alt text](1.2_netcat_and_privilege_verification.png)

Observation

A direct connection was successfully established to the service exposed on Port 1524.

Following connection, identity-verification commands returned:

root

and

uid=0(root) gid=0(root)

confirming that the service was operating under the highest privilege level available on the target system.

Analysis

Manual interaction validated the results obtained during service enumeration and confirmed that the exposed service was functional and accessible.

The privilege-verification results demonstrated that interactions with the service occurred with administrative permissions. From a security perspective, services operating with elevated privileges represent a significant risk because successful access may provide unrestricted visibility into host resources, processes, configurations, and authentication data.

This stage highlights the relationship between:


Service Discovery | 
Service Validation |
Privilege Verification | 
Security Impact Assessment

The assessment therefore established Port 1524 as a critical entry point requiring further investigation during the subsequent phases of system reconnaissance and credential analysis.

## Phase 1 Conclusion

Analysis of TCP Port 1524 revealed that the target system exposed a network-accessible shell service. Service enumeration successfully identified the nature of the service, while manual validation confirmed direct access to the target operating system.

Privilege verification demonstrated that interactions with the service occurred under administrative privileges, substantially increasing the potential security impact of the exposed service.

The findings obtained during this phase establish the foundation for the next stage of the assessment, where internal reconnaissance activities will be performed to understand the target environment, identify running services, and locate credential-related artifacts.