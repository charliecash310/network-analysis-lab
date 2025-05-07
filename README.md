# Network-Analysis-Lab

# Tools

🐬 Wireshark – Packet analysis
🧪 iperf3 – Bandwidth and latency testing
📜 PowerShell – Automate ping and trace routes
🖥️ Two VMs or physical machines for testing (preferred but optional)

# Setup iperf3 for Bandwidth Testing

# ⚙️ On one system (server):
iperf3 -s

# ⚙️ On the other system (client):
iperf3 -c <server_ip>

# Save the result output to:
/tests/iperf3_test_YYYYMMDD.txt

# Optional flags to add:
iperf3 -c <server_ip> -t 10 -i 1

# Wireshark Capture 
Capture a session:
1. Start Wireshark on one machine.

2. Filter for traffic: ip.addr == <other_machine_ip>

3. Run network-intensive tasks (speed test, file copy, ping).

4. Stop capture and save as:

/captures/test_capture_YYYYMMDD.pcapng
