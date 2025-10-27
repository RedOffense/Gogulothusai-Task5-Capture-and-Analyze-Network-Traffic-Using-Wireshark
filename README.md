# Gogulothusai-Task5-Capture-and-Analyze-Network-Traffic-Using-Wireshark

# 🧑‍💻Capture and Analyze Network Traffic Using Wireshark on Linux
This project involves the capture, analysis, and interpretation of network traffic using Wireshark, with a focus on identifying and examining common network protocols over a eth0 connection 
The report is structured to present objectives, tools used, methodology, protocol details, packet structures, observations, and final conclusions.

The goal is to enhance practical knowledge of network communication, packet structures, and protocol interactions while maintaining ethical standards and privacy compliance.

# Basic Contents For wireshark Use 

# 1.Install & setup
  * Windows: Download Wireshark from wireshark.org and install. Choose Npcap when prompted (required for live capture).
  * macOS: Download the .dmg from wireshark.org (or use Homebrew brew install --cask wireshark), allow the helper tool.
  * Linux: Use your package manager (sudo apt install wireshark on Debian/Ubuntu; ensure dumpcap has proper permissions). On many systems you’ll add your user to the wireshark group or use sudo to capture.
  * Remote capture: Use ssh, rpcap, or dumpcap on the remote machine to generate .pcap files you analyze locally.
  * Permissions: Capturing requires elevated privileges. On Linux, best practice: allow dumpcap to capture (setcap) instead of running full GUI as root.

# 2.Basic capture workflow (GUI)
  * Open Wireshark → Capture → Options.
  * Pick the interface 'Ethernet'
  * Optionally set a capture filter (applies while collecting; reduces disk use).
       host 10.0.0.5 or tcp port 443.
  * Click Start to capture, reproduce the problem, then Stop.

# Capture vs display filters (critical distinction)
  * Capture filters (applied during capture)
      *      udp or dns
      *      tcp port 80
  * Display filters (applied after capture): powerful for inspecting stored packets.
      *      http
      *      ip.src == 192.168.1.10 && tcp.dstport == 443
      *      ip.addr == ip address
      *      tcp.analysis.retransmission
   
# Useful display filters (cheat sheet)
  * tcp— all TCP traffic
  * udp — all UDP traffic
  * ip.addr == 10.0.0.5 — any packet with that IP as source
  * ip.src == 10.0.0.5 — source IP
  * tcp.port == 80 or tcp.dstport == 443
  * http — HTTP protocol packets
  * dns — DNS packets
  * tls — TLS (SSL) packets
  * icmp — ICMP (ping)
  * tcp.flags.syn == 1 && tcp.flags.ack == 0

# Common troubleshooting steps & examples
**A. High latency or retransmits**
  * Filter: tcp.analysis.retransmission || tcp.analysis.fast_retransmission || tcp.analysis.ack_lost_segment
  * Look for large RTTs: tcp.time_delta fields in TCP details / Statistics → TCP Stream Graph → Round Trip Time.
 
**B. DNS issues**
  * Filter: dns
  * Check responses vs queries, response codes, or delays.
    
**C. Slow web page load**
  * Capture browser-to-server conversation, then: Follow TCP stream (HTTP) → see request headers, status codes, content length.
  * For HTTPS, use TLS decryption (next section) if possible.
    
**D. Service unreachable**      
  * Check ICMP for Destination Unreachable: icmp
  * Check TCP 3-way handshake: filter SYNs tcp.flags.syn==1 && tcp.flags.ack==0

# Exporting & reporting

  * Save captures: File → Save As (use .pcapng).
  * Export packets: selected packets → File → Export Specified Packets.
  * Export packet bytes or summary CSV: File → Export Packet Dissections → As CSV/plaintext.
  * For reports, include: objective, capture time range, interfaces, capture filters used, key findings (packets/times), screenshots of follow-streams or graphs, and recommended remediation.

# Ethical & legal considerations (must-read)

  * Only capture traffic you own or have explicit permission to capture. Capturing others’ traffic may be illegal.

  * Decrypting encrypted traffic requires consent. Don’t attempt to decrypt traffic without authorization.

  * When in a corporate environment, follow policy and involve security/network teams.

# Resources to learn more

  * Wireshark User’s Guide (built into Help menu).

  * Built-in sample captures (Help → Sample Captures) — great for practice.

  * Official Wireshark wiki and documentation (wireshark.org/docs) — if you want deeper protocol decodes.

# Summarize & Classify Traffic

**Wireshark can automatically summarize protocol usage.**

    Statistics → Protocol Hierarchy
 This shows what percentage of traffic is TCP, UDP, HTTP, DNS, etc.

# Privacy and Ethical Considerations

  * No personal or sensitive data was captured, stored, or shared.
  * Analysis was performed only on authorized network traffic.
  * All findings have been sanitized to remove identifying information.
  * This work follows ethical guidelines for responsible network monitoring.
 
