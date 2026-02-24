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

### Step 1 – Open the PCAP in Wireshark
![Step 1](images/1.png)

### Step 2 – Identify TCP Handshake
![Step 2](images/2.png)

### Step 3 – Inspect HTTP GET Request
![Step 3](images/3.png)

### Step 4 – Follow TCP Stream
![Step 4](images/4.png)

### Step 5 – Review Binary Data
![Step 5](images/5.png)

### Step 6 – Identify Embedded Strings
![Step 6](images/6.png)

### Step 7 – Confirm Real Executable
![Step 7](images/7.png)

### Step 8 – Export HTTP Object
![Step 8](images/8.png)

### Step 9 – Save the Extracted File
![Step 9](images/9.png)

### Step 10 – Verify the Saved File
![Step 10](images/10.png)

### Step 11 – Analyze File Type
![Step 11](images/11.png)

### Step 12 – Inspect PE Structure
![Step 12](images/12.png)

### Step 13 – Review Imported Libraries
![Step 13](images/13.png)

### Step 14 – Examine Suspicious Strings
![Step 14](images/14.png)

### Step 15 – Final Analysis & Confirmation
![Step 15](images/15.png)
---
