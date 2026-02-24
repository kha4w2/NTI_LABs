---

# Lab 27.2.14 – Extract an Executable from a PCAP

## Cisco CyberOps Associate

## Overview

In this lab, I performed packet-level network analysis to reconstruct and extract an executable file from a captured PCAP file. The objective was to understand how file downloads occur over HTTP, how TCP streams can be rebuilt, and how analysts can recover transferred files directly from packet captures.

The analysis was conducted using the CyberOps Workstation virtual machine and Wireshark.

---

## Part 1 – Traffic Analysis

The file `nimda.download.pcap` was opened in Wireshark.

### TCP and HTTP Analysis

* The first three packets represented the TCP three-way handshake.
* The fourth packet contained an HTTP GET request for a file named:

  `W32.Nimda.Amm.exe`

This confirmed that the client initiated a file download over HTTP.

Using **Follow TCP Stream**, I reconstructed the entire TCP conversation between the client and server.

### Binary Data Interpretation

Inside the TCP stream:

* Unreadable symbols appeared because the transferred file was a binary executable.
* These symbols represent actual file content, not connection noise.
* Readable words embedded in the stream were executable strings stored inside the binary.

Upon reviewing the embedded strings, the executable was identified as:

**Microsoft Windows Command Processor (cmd.exe)**

Although the file was named `W32.Nimda.Amm.exe`, analysis showed that it was actually the legitimate Windows `cmd.exe` file that had been renamed. This demonstrates a masquerading technique, where a file is renamed to appear malicious.

---

## Part 2 – File Extraction

Using:

**File → Export Objects → HTTP**

Wireshark displayed the HTTP objects contained within the TCP stream. Only one file was present in the capture because the packet recording started immediately before the download and stopped right after it completed.

The file was exported and saved to the `/home/analyst` directory.

### Verification

Using the command:

```bash
ls -l
```

The file was confirmed to be successfully saved with a size of 345,088 bytes.

To identify the file type, the following command was executed:

```bash
file W32.Nimda.Amm.exe
```

Output:

```
PE32+ executable (console) x86-64, for MS Windows
```

This confirmed that the extracted object was a Windows Portable Executable file.

---

## Conclusion

In this lab, I:

* Analyzed TCP handshake and HTTP traffic
* Reconstructed a full TCP stream
* Identified a file download at the packet level
* Extracted an executable directly from a PCAP file
* Performed basic static inspection of the file

The lab demonstrated how network forensic analysis can recover transferred files and verify their true identity, rather than relying solely on filenames.

---

## Steps

<img width="1918" height="1010" alt="1" src="https://github.com/user-attachments/assets/02aaf7f8-c76f-475b-a121-412ac0e3511c" />

<img width="1017" height="108" alt="2" src="https://github.com/user-attachments/assets/589b4b91-79ff-4627-b3c8-c6f4821e3f8b" />

<img width="1918" height="1001" alt="3" src="https://github.com/user-attachments/assets/d85d3f84-cec9-4d82-9dd0-c1c15b264b72" />


## Steps

<p align="center">
  <img src="images/1.png" width="900"/><br><br>
  <img src="images/2.png" width="900"/><br><br>
  <img src="images/3.png" width="900"/><br><br>
  <img src="images/4.png" width="900"/><br><br>
  <img src="images/5.png" width="900"/><br><br>
  <img src="images/6.png" width="900"/><br><br>
  <img src="images/7.png" width="900"/><br><br>
  <img src="images/8.png" width="900"/><br><br>
  <img src="images/9.png" width="900"/><br><br>
  <img src="images/10.png" width="900"/><br><br>
  <img src="images/11.png" width="900"/><br><br>
  <img src="images/12.png" width="900"/>
</p>


---
