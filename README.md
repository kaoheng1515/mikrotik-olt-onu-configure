
# Huawei OLT Configuration Guide for GPON FTTH with MikroTik
Based on your previous setup (MikroTik as core router with VLAN-per-customer for PPPoE billing), here's a complete guide for Huawei OLT integration. This focuses on the popular MA5608T model (most common in Cambodia for small/medium ISPs in 2025, ~$800–$1,200 USD). It supports up to 8 PON ports (512 ONUs per port, total ~4,000 customers).
Huawei OLTs are reliable for GPON but more complex than BDCOM—use CLI (Telnet/Console) or U2000 NMS for management. Default login: root/admin or root/admin123. Connect via console (9600 baud) initially.
