# Putty Patrol Malware Analysis

**Author:** Eszter Iszlai & Stefan Krijt  
**Date:** 2026-07-24  
**Classification:** CONFIDENTIAL  

---

## Overview

This project documents the complete analysis of a malicious Windows executable (`putty.exe`) masquerading as the legitimate PuTTY SSH client. The sample functions as a dropper, extracting and executing a hidden PowerFun reverse shell payload over SSL-encrypted C2 communication.

**Key Deliverables:**
- Full static and dynamic analysis report (PDF)
- Custom YARA rule for threat hunting
- Indicators of Compromise (IOCs) in machine-readable format
- Extracted PowerShell payload and FLOSS string output

**Threat Intelligence:**  
- **SHA256:** `0C82E654C09C8FD9FDF4899718EFA37670974C9EEC5A8FC18A167F93CEA6EE83`  
- **C2 Domain:** `bonus2.corporatebonusapplication.local`  
- **C2 Port:** `8443`  
- **Threat Family:** PowerFun / Meterpreter  

**Use Case:**  
This repository is intended for security analysts, incident responders, and threat intelligence teams to aid in detection and mitigation of this malware family.

---

## Sample Summary

| Property | Value |
|----------|-------|
| **Sample Name** | `putty.exe` |
| **SHA256** | `0C82E654C09C8FD9FDF4899718EFA37670974C9EEC5A8FC18A167F93CEA6EE83` |
| **File Type** | PE32 executable (GUI) Intel 80386 |
| **Threat Classification** | Trojan / Dropper / Reverse Shell |
| **Threat Family** | PowerFun |
| **Overall Risk** | **HIGH** |

---

## Key Findings

- The sample is a **malicious 32-bit Windows executable** masquerading as legitimate PuTTY software.
- It acts as a **dropper**, extracting and executing a hidden **PowerShell reverse shell payload**.
- The payload connects to a hardcoded C2 domain (`bonus2.corporatebonusapplication.local`) over **SSL-encrypted TCP port 8443**.
- The malware establishes **persistence** via registry modifications and can execute arbitrary commands on the compromised host.
- **VirusTotal:** 46/56 detections (Trojan.Rozena/Meterpreter).
- **Hybrid Analysis:** Threat Score 100/100 with 231 matched indicators.
- **YARA Rule:** Successfully created and tested; can detect this malware family.

---

## Repository Contents

| File | Description |
| :--- | :--- |
| `/Report/Putty_Patrol_Analysis_Report.pdf` | Full analysis report, including static and dynamic findings. |
| `/IOCs/iocs.txt` | Machine-readable list of IOCs. |
| `/IOCs/detect_malware.yar` | YARA rule for detecting this malware family. |
| `/Artifacts/strings_output.txt` | Full FLOSS string extraction output (20,161 strings). |
| `/Artifacts/payload_decompressed.txt` | Decoded and decompressed PowerShell reverse shell script. |
| `/sample_hash.txt` | SHA256 hash of the malware sample. |

---

## Key Indicators of Compromise (IOCs)

| Type | Value |
| :--- | :--- |
| **SHA256** | `0C82E654C09C8FD9FDF4899718EFA37670974C9EEC5A8FC18A167F93CEA6EE83` |
| **C2 Domain** | `bonus2.corporatebonusapplication.local` |
| **C2 Port** | `8443` |
| **Filename** | `putty.exe` |
| **PowerShell Prefix** | `H4sIAOW/UWECA51W227` |
| **Function Names** | `powerfun`, `Get-Webclient` |
| **Suspicious Imports** | `CreateProcessA`, `RegCreateKeyExA`, `ShellExecuteA`, `OpenProcess`, `WriteFile` |

---

## Usage

### YARA Rule

To scan for this malware:

```cmd
yara64.exe -r detect_malware.yar putty.exe
```

**Expected Output:**
```
Malware_PuTTY_Dropper_PowerFun putty.exe
```

### Threat Hunting

Search for the C2 domain (`bonus2.corporatebonusapplication.local`) or the SHA256 hash in your network logs or SIEM.

### Network Blocking

- Block outbound connections to `bonus2.corporatebonusapplication.local` on port `8443`.
- Monitor for `powershell.exe` spawning with hidden, encoded Base64 commands.

---

## Contact

For questions or additional information, please contact the authors.

---

## Disclaimer

This repository and its contents are provided for **educational and defensive purposes only**. The authors are not responsible for any misuse of the information contained herein.

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.
