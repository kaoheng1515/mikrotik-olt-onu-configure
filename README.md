
# Huawei OLT Configuration Guide for GPON FTTH with MikroTik
Based on your previous setup (MikroTik as core router with VLAN-per-customer for PPPoE billing), here's a complete guide for Huawei OLT integration. This focuses on the popular MA5608T model (most common in Cambodia for small/medium ISPs in 2025, ~$800–$1,200 USD). It supports up to 8 PON ports (512 ONUs per port, total ~4,000 customers).
Huawei OLTs are reliable for GPON but more complex than BDCOM—use CLI (Telnet/Console) or U2000 NMS for management. Default login: root/admin or root/admin123. Connect via console (9600 baud) initially.

## Features
- **Simple Queue + Queue Tree:** Limit speed per customer exactly (10/10, 20/5, 50/50 Mbps…)
- **Universal Queue (PCQ):** Fair sharing when line is full → no one hogs all bandwidth
- **Cake Queue (RouterOS 7.13+):** YBest for bufferbloat, very smooth Zoom/YouTube even on full line
- **ScalabPPPoE + MikroTik APIility:**  Connect to local billing software (CamISP, MikroBill, etc.)
- **Torch & Packet Sniffer:**  Find who downloads torrent 24h very fast
- **VLAN per customer**  No broadcast storm, very secure

```bash
flowchart LR

    A["MikroTik Router<br>ether2 (Trunk)<br>VLAN 100 = Internet<br><br>IP: 192.0.2.1/24<br>Gateway: OLT 192.0.2.2"]
    B["Huawei OLT<br>Uplink GE0/1 (Trunk)<br>PON 0/1/1<br>Service-Port VLAN 100"]
    C["ONU (Bridge Mode)<br>UNI Port 1<br>Customer CPE<br><br>Gets IP from MikroTik DHCP"]
    D["Internet<br>ISP Gateway"]

    %% FLOW
    A -->|"Tagged VLAN 100<br>Data/PPPoE/DHCP"| B
    B -->|"Maps to Service-Port<br>Send to PON<br>PON0/1/1 → ONU1"| C
    C -->|"Customer traffic"| B -->|"Uplink (VLAN 100)"| A
    A -->|"NAT / Routing"| D
    D -->|"Return Traffic"| A

    %% COLOR STYLES
    classDef mk fill:#dc2626, color:#fff
    classDef olt fill:#2563eb, color:#fff
    classDef onu fill:#16a34a, color:#fff
    classDef internet fill:#0ea5e9, color:#fff

    class A mk
    class B olt
    class C onu
    class D internet
```