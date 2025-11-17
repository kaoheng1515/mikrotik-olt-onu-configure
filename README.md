
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

## Works Flow

```mermaid
 flowchart TD
    Internet[Internet<br>Real Public IP or CGNAT] -->|Fiber Cable| MK[MikroTik Router<br>CCR2004 / RB4011<br><br>SPF1 = WAN<br>SPF2 = Trunk to OLT<br>All customer VLANs tagged]
    
    MK -->|VLAN 100 tagged<br>DHCP Server on vlan100<br>IP pool 192.168.100.0/24| OLT[Huawei MA5608T OLT<br>GE0/1/0 = Trunk uplink<br>Allow VLAN 100 and VLAN 999 mgmt<br>Management IP 192.168.99.2]
    
    OLT -->|PON 0/1 -> 0/8<br>Service-port VLAN 100<br>Tag-transform transparent| SPL[Splitter 1:32 or 1:64]
    
    SPL --> ONU1[ONU #1<br>HG8546M / V-SOL<br>Bridge Mode<br>LAN1 -> Customer Router<br>Gets 192.168.100.x from MikroTik]
    SPL --> ONU2[ONU #2<br>Same VLAN 100]
    SPL -->|...| ONU64[ONU #64<br>Max 64 per PON]
    
    ONU1 --> Customer1[Customer Wi-Fi Router<br>or PC Direct]

    style MK fill:#dc2626,color:white
    style OLT fill:#2563eb,color:white
    style ONU1 fill:#16a34a,color:white
    style Internet fill:#0ea5e9,color:white
```