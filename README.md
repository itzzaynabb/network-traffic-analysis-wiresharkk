# Network Traffic Analysis & Protocol Inspection (Wireshark)

## 📌 Project Overview
As part of practical Blue Team and defensive security training, this project examines live and simulated network traffic to understand protocol behavior, connection handshakes, and the security risks associated with unencrypted communications.

---

## 🛠️ Tools & Environment
- **Packet Analyzer:** Wireshark v4.6
- **Capture Driver:** Npcap
- **Operating System:** Windows 10/11
- **Target Protocols:** TCP, HTTP, DNS

---

## 🔍 Investigation & Analysis

### 1. TCP Three-Way Handshake Analysis
Using Wireshark display filters (`tcp.stream eq [X]`), isolated a complete TCP session establishing a connection between the client (`192.168.100.20`) and an external server (`34.223.124.45` on port 80).
- **SYN (Client -> Server):** Client initiates connection and synchronizes sequence numbers.
- **SYN-ACK (Server -> Client):** Server acknowledges the request and sends its synchronization flag.
- **ACK (Client -> Server):** Client completes the handshake, transitioning the connection to `ESTABLISHED`.

### 2. HTTP Cleartext Protocol Inspection
Applied the `http` display filter to evaluate unencrypted Hypertext Transfer Protocol traffic.
- **Request Inspection:** Identified `GET / HTTP/1.1` requests displaying plain-text Host headers, client `User-Agent` strings, and connection parameters.
- **Stream Reconstruction:** Utilized Wireshark's **Follow TCP Stream** feature to reconstruct the full bi-directional conversation between client and server.
- **Security Assessment:** Confirmed that legacy HTTP lacks cryptographic protection. Any credentials, session tokens, or sensitive payloads transmitted over port 80 are vulnerable to packet sniffing and Man-in-the-Middle (MitM) interception.

<img width="1293" height="763" alt="Screenshot 2026-09-17 154824" src="https://github.com/user-attachments/assets/94ba65c0-80b6-4527-93e4-47a36fa077af" />


## 🛡️ Key Takeaways & Defensive Recommendations
1. **Enforce Transport Layer Security (TLS/HTTPS):** Disable cleartext HTTP for web applications and redirect port 80 to port 443.
2. **Implement HSTS (HTTP Strict Transport Security):** Prevent protocol downgrade attacks by instructing browsers to exclusively interact over HTTPS.
3. **SOC Monitoring Value:** Understanding baseline handshake sequences and packet headers is critical for identifying anomalies such as SYN floods, port scans, and data exfiltration over cleartext protocols.
