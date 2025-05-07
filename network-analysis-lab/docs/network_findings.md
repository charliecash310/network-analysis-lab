# Network Traffic Analysis Report

## Test Summary
- Date: May 4th, 2025
- Tool: iperf3
- Machines: Ubuntu Client → Windows Server

## iperf3 Summary:
## Key Observations:
- Total Data Transferred: 2.97 GB
- Average Bandwidth: 2.54 Gbit/sec
- Peak Bandwidth: 2.88 Gbit/sec

- Observation: The connection was stable and consistently high-performing with no indication of retransmissions or packet loss. 
  The last interval showed a partial transfer due to test termination, which is expected behavior.
- Transfer of 2.97 Gigabytes of data at a Bitrate of 2.54 Gigabytes/sec
- Fairly consistent output between 2.0-2.88 Gbit/sec across most seconds.

## Wireshark Findings:
- TCP handshake success
- Several [TCP Dup ACKs] and [TCP Retransmissions]
- Please see images/raw file

## Actionable Recommendations:
- Switch to wired if using wireless
- Investigate firewall/DNS routing issues

############################################################################

## WIRESHARK ANALYSIS

# 📡 Network Traffic Analysis Report (iperf3 TCP Test)

**Date:** May 4, 2025  
**Tool:** Wireshark  
**Capture File:** `iperf3_test_linux_2_20250504.pcapng`  
**Interface Used:** `enp0s8` (Linux Internal Adapter)  
**Filter Applied:** `ip.addr == 10.10.10.20 && tcp.port == 5201`

---

## 🧪 Test Setup

| Component     | Details              |
|---------------|----------------------|
| Client        | Windows 10 (`10.10.10.20`) |
| Server        | Ubuntu Linux (`10.10.10.50`) |
| Protocol      | TCP                  |
| Port          | 5201 (iperf3 default) |
| Duration      | 10 seconds           |

The test simulated a file transfer over a direct connection using `iperf3` to evaluate throughput and traffic behavior.

---

## 📊 Key Packet Observations

| Packet Behavior       | Summary |
|------------------------|---------|
| **TCP Handshake**     | Successful: SYN → SYN-ACK → ACK |
| **Throughput**        | Steady packet stream with ~2.5 Gbit/sec transfer |
| **Window Scaling**    | Enabled, adapting to bandwidth |
| **Retransmissions**   | None observed (clean link) |
| **Dup ACKs**          | None observed |
| **Connection Close**  | Proper FIN/ACK handshake seen |

---

## 📸 Screenshot

![Wireshark TCP Stream Screenshot]()

*Shows consistent TCP transfer with no packet loss.*

---

## ✅ Conclusion

The internal network (`labnet`) between the two VMs demonstrates reliable and high-speed throughput, with no signs of congestion, retransmissions, or jitter. This environment is well-suited for isolated traffic analysis and simulating LAN performance scenarios.

---

## 🔁 Next Steps

- Test UDP throughput with `iperf3 -u`
- Capture DNS or HTTP traffic
- Analyze network latency under load

-------------------------------------------------------------------------------

## 🔧 Simulated Packet Loss Test

**Command Used:**
bash
sudo tc qdisc add dev enp0s8 root netem delay 100ms 20ms loss 5%
-------------------------------------------------------------------------------
Accepted connection from 10.10.10.20, port 61971
[  5] local 10.10.10.50 port 5201 connected to 10.10.10.20 port 61972
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec   444 KBytes  3.64 Mbits/sec                  
[  5]   1.00-2.00   sec  2.84 MBytes  23.9 Mbits/sec                  
[  5]   2.00-3.02   sec  2.13 MBytes  17.6 Mbits/sec                  
[  5]   3.02-4.00   sec  1.83 MBytes  15.6 Mbits/sec                  
[  5]   4.00-5.00   sec  1.62 MBytes  13.6 Mbits/sec                  
[  5]   5.00-6.00   sec  1.84 MBytes  15.4 Mbits/sec                  
[  5]   6.00-7.00   sec  1.96 MBytes  16.4 Mbits/sec                  
[  5]   7.00-8.00   sec  1.93 MBytes  16.2 Mbits/sec                  
[  5]   8.00-9.00   sec  1.98 MBytes  16.6 Mbits/sec                  
[  5]   9.00-10.00  sec  2.01 MBytes  16.9 Mbits/sec                  
[  5]  10.00-10.12  sec   207 KBytes  13.6 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.12  sec  18.8 MBytes  15.6 Mbits/sec                  receiver

-------------------------------------------------------------------------------
Accepted connection from 10.10.10.20, port 61338
[  5] local 10.10.10.50 port 5201 connected to 10.10.10.20 port 61339
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec   312 MBytes  2.62 Gbits/sec                  
[  5]   1.00-2.00   sec   231 MBytes  1.93 Gbits/sec                  
[  5]   2.00-3.00   sec   276 MBytes  2.32 Gbits/sec                  
[  5]   3.00-4.00   sec   270 MBytes  2.27 Gbits/sec                  
[  5]   4.00-5.00   sec   231 MBytes  1.94 Gbits/sec                  
[  5]   5.00-6.00   sec   221 MBytes  1.85 Gbits/sec                  
[  5]   6.00-7.00   sec   304 MBytes  2.55 Gbits/sec                  
[  5]   7.00-8.00   sec   284 MBytes  2.38 Gbits/sec                  
[  5]   8.00-9.00   sec   256 MBytes  2.14 Gbits/sec                  
[  5]   9.00-10.00  sec   197 MBytes  1.66 Gbits/sec                  
[  5]  10.00-10.07  sec  7.51 MBytes   887 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.07  sec  2.53 GBytes  2.16 Gbits/sec                  receiver
-------------------------------------------------------------------------------
#Effect Observed:
- Average latency increased to ~120ms
- Packet loss observed in Wireshark
- Throughput dropped from 2.5 Gbit/s to ~1.2 Gbit/s
- TCP retransmissions detected in the capture