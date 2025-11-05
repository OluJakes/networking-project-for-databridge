# 🖧 Computer Networking – Access Control List (ACL) Implementation in Cisco Packet Tracer

## 📘 Project Overview
This project demonstrates how **Access Control Lists (ACLs)** can be used on Cisco routers to regulate inter-departmental access in a network. The goal is to **permit Admin and Sales** departments to reach the company web server, while **restricting HR** access. This showcases how ACLs enhance **network security**, **access control**, and **traffic management**.

---

## 🧩 Network Design
- **Router:** Cisco 2911  
- **Switches:** Cisco 2960 (Admin, Sales, HR)  
- **Server:** Web Server at `192.168.10.5`  
- **Subnets:**
  - Admin: `192.168.10.0/24` → Gateway: `192.168.10.1`
  - Sales: `192.168.20.0/24` → Gateway: `192.168.20.1`
  - HR: `192.168.30.0/24` → Gateway: `192.168.30.1`

Each subnet connects to the router via a dedicated interface. ACLs are applied inbound on Sales and HR interfaces to enforce access restrictions.

---

## ⚙️ Configuration Summary
```bash
! Router Configuration
enable
configure terminal
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
interface GigabitEthernet0/2
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit

! ACL Configuration
ip access-list extended HTTP-SERVER-ACCESS
 permit ip 192.168.10.0 0.0.0.255 host 192.168.10.5
 permit ip 192.168.20.0 0.0.0.255 host 192.168.10.5
 deny ip 192.168.30.0 0.0.0.255 host 192.168.10.5
 permit ip any any
exit

! Apply ACLs
interface GigabitEthernet0/1
 ip access-group HTTP-SERVER-ACCESS in
exit
interface GigabitEthernet0/2
 ip access-group HTTP-SERVER-ACCESS in
exit
end
write memory
