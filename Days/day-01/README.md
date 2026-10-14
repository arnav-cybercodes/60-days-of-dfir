# DFIR Daily Learning — Day 1
## Case Study: Ransomware Again... But We Changed the RDP Port!?!

**Source:** The DFIR Report  
**Original Report:** https://thedfirreport.com/2020/07/13/ransomware-again-but-we-changed-the-rdp-port/  
**Focus Areas:** Initial Access, RDP, Network Discovery, Lateral Movement, Windows Forensics, Ransomware

---

## 1. Objective

The objective of this exercise was to analyze a real-world ransomware incident and understand how an attacker gained initial access, discovered internal systems, accessed a Domain Controller, and deployed ransomware.

This is a report-based learning exercise, not an independent forensic investigation.

## 2. Incident Summary

The attacker gained access to a Windows system through Remote Desktop Protocol (RDP) using brute-forced credentials. Despite RDP being configured on a non-default port, the system was compromised.

After gaining access, the attacker performed internal network discovery, identified additional systems, accessed a Domain Controller, and deployed Harma ransomware.

The incident demonstrates that changing the default RDP port alone is not an effective security boundary.

## 3. Attack Timeline

### Stage 1: Initial Access
- The attacker accessed the initial Windows system through RDP.
- The report attributes the initial compromise to RDP brute force on a non-default port.
- The exact method used to discover the port is not established by the timeline alone.

### Stage 2: Internal Network Discovery
- The attacker used a network-scanning tool to discover reachable internal systems.
- Network discovery helped identify additional potential targets.

### Stage 3: Domain Controller Access
- The attacker subsequently connected to a Domain Controller using RDP.
- The precise method used to discover the DC's RDP port and obtain or reuse credentials is not established by the timeline alone.

### Stage 4: Further Discovery
- Additional network discovery activity occurred.
- The attacker continued exploring the environment before deploying ransomware.

### Stage 5: Ransomware Deployment
- Harma ransomware was dropped and executed on the Domain Controller.
- The ransomware was subsequently dropped and executed on the original entry-point machine.
- The resulting activity affected files accessible to the compromised systems.

*Note: This timeline summarizes the published investigation. It does not establish every intermediate action or the exact mechanism behind every access event.*

## 4. Key Technical Concepts

### Remote Desktop Protocol (RDP)
RDP allows users to access Windows systems remotely.

Changing the default TCP port 3389 to a non-standard port may reduce noise from automated scans targeting the default port, but it does not prevent port discovery or protect weak and compromised credentials.

### Network Discovery
Network-scanning tools can identify reachable hosts and services. The results can help an attacker locate potential targets, including servers and Domain Controllers.

A scan alone does not prove that a host is a Domain Controller or that the attacker has credentials to access it.

### Domain Controller (DC)
A Domain Controller manages authentication and directory services in a Windows domain. Unauthorized access to a DC can put a large portion of an organization's environment at risk.

### Ransomware
Ransomware encrypts files to disrupt access to data. Its actual impact depends on the privileges of the compromised account, accessible files and shares, and the ransomware's capabilities.

## 5. Defensive Investigation Opportunities

| Activity | Evidence to Investigate |
|---|---|
| RDP logon | Windows Security Event ID 4624; examine logon type and source address |
| Failed logons | Windows Security Event ID 4625 |
| Process execution | Security Event ID 4688 or Sysmon Event ID 1, if logging is configured |
| Network discovery | Endpoint process telemetry, Sysmon network events, firewall logs, and network monitoring |
| Remote access to a DC | DC logon records, source host, account, and session timing |
| Ransomware execution | Process telemetry, file-system changes, suspicious executable metadata, and endpoint alerts |

These are potential investigation sources, not a claim that every listed log or artifact was available in this incident.

## 6. Key Lessons Learned

1. Changing the default RDP port is not a substitute for strong authentication and access restrictions.
2. A successful initial compromise can lead to internal network discovery and further access.
3. A Domain Controller is a high-value target because it provides critical domain authentication services.
4. Investigators must distinguish observed activity from hypotheses about how an attacker acted.
5. A timeline helps correlate logons, process execution, network activity, and ransomware deployment.
6. Detection depends on the logs and telemetry actually collected by the organization.

## 7. Questions for Further Research

- How can investigators establish the initial source of an RDP login?
- What evidence can identify a network-scanning tool and the hosts it discovered?
- How can investigators determine whether the same credentials were reused across systems?
- Which artifacts can help confirm ransomware execution?
- How can MFA, VPN access, account restrictions, and endpoint monitoring reduce the risk of RDP compromise?

## 8. Limitations

This case study is based on a publicly available incident report. The complete underlying forensic evidence was not independently examined.

Where the report does not establish a precise mechanism, the conclusion is recorded as unknown rather than assumed.

## 9. Reference

- The DFIR Report — [Ransomware Again... But We Changed the RDP Port!?!?](https://thedfirreport.com/2020/07/13/ransomware-again-but-we-changed-the-rdp-port/)

---

**Learning Progress:** Day 1 completed  
**Next Focus:** Remote access tools, attacker discovery, credential access, lateral movement, and data exfiltration.
