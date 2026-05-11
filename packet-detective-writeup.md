# PacketDetective Lab Walkthrough

## Overview
PacketDetective is a network forensics challenge from CyberDefenders that focuses on analyzing suspicious SMB traffic using Wireshark. The objective of this lab is to investigate packet captures (PCAPs), identify attacker activities, and extract Indicators of Compromise (IOCs).

---

# Tools Used
- Wireshark
- PCAP Files
- SMB & DCERPC Protocol Analysis

---

# Traffic-1.pcapng Analysis

## Question 1 — Total SMB Bytes

### Steps
1. Open the PCAP file in Wireshark.
2. Navigate to:
   ```text
   Statistics → Protocol Hierarchy
   ```
3. Locate the SMB protocol statistics.

### Answer
```text
4406 bytes
```

---

## Question 2 — SMB Authentication Username

### Steps
Apply the following filter:

```text
ntlmssp.auth.username
```

Inspect the authentication packets to identify the username.

### Answer
```text
Administrator
```

---

## Question 3 — File Accessed by the Attacker

### Steps
Apply the filter:

```text
smb.cmd == 0xA2
```

Inspect SMB Trans2 requests to identify the accessed file.

### Answer
```text
eventlog
```

---

## Question 4 — Event Log Clearing Timestamp

### Steps
1. Apply the filter:
   ```text
   dcerpc.opnum == 0
   ```

2. Search for:
   ```text
   ClearEventLogW
   ```

3. Change the time display format:
   ```text
   View → Time Display Format → UTC Date and Time of Day
   ```

### Answer
```text
2020-09-23 16:50:16 UTC
```

---

# Traffic-2.pcapng Analysis

## Question 5 — Named Pipe Service

### Steps
Apply the filter:

```text
dcerpc
```

Inspect the packet details and search for the keyword:

```text
pipe
```

### Answer
```text
atsvc
```

---

## Question 6 — Communication Duration

### Steps
1. Navigate to:
   ```text
   Statistics → Conversations → IPv4
   ```

2. Locate communication between:
   ```text
   172.16.66.1 ↔ 172.16.66.36
   ```

### Answer
```text
11.7247 seconds
```

---

# Traffic-3.pcapng Analysis

## Question 7 — Suspicious Username

### Steps
Review authentication packets and suspicious SMB activity to identify the attacker-created username.

### Answer
```text
Backdoor
```

---

## Question 8 — Remote Execution File

### Steps
1. Inspect SMB and service creation traffic.
2. Search packet details for executable names.

### Answer
```text
psexesvc.exe
```

---

# Key Takeaways

- SMB traffic can reveal authentication attempts and attacker lateral movement.
- Event log clearing is a common anti-forensics technique.
- Named pipes and DCERPC communication are useful indicators during investigations.
- Tools like PsExec are commonly used for remote execution during attacks.
- Wireshark filtering is essential for efficient packet analysis in SOC investigations.

---

# Skills Gained

- Network Traffic Analysis
- SMB Protocol Investigation
- Wireshark Filtering
- Incident Investigation
- IOC Identification
- SOC Analyst Fundamentals
