# 🖧 Computer Networking – Access Control List (ACL) Implementation in Cisco Packet Tracer

## 📘 Project Overview
This project demonstrates the configuration of **Access Control Lists (ACLs)** on a Cisco router to manage inter-departmental network communication. The setup ensures that **Admin** and **Sales** departments can access a company web server, while the **HR** department is denied access — showcasing practical implementation of security policies in enterprise networking.
---

## 🔗 View Project 👉 [View Full Documentation (PDF)](https://drive.google.com/file/d/10NLI6gIRDUCjiJOM4z5BdKHEr8Q2gYlR/view?usp=sharing)
---

## 🎯 Project Objectives
- To understand how **ACLs** are used to filter network traffic based on IP addresses and protocols.  
- To configure a **Cisco router** for departmental access segmentation.  
- To implement **security controls** that prevent unauthorized access to critical resources.  
- To test, verify, and document the impact of ACL configurations on inter-network communication.

---

## 🧩 Network Design
- **Router:** Cisco 2911  
- **Switches:** Cisco 2960 (Admin, Sales, HR)  
- **Server:** Web Server – IP `192.168.10.5` (Admin Department)  
- **Subnet Details:**
  - Admin → `192.168.10.0/24` (Gateway: `192.168.10.1`)
  - Sales → `192.168.20.0/24` (Gateway: `192.168.20.1`)
  - HR → `192.168.30.0/24` (Gateway: `192.168.30.1`)

**Design Summary:**  
Each department is assigned a unique subnet, connected via dedicated switches to the main router. ACLs are applied on inbound interfaces to enforce role-based access control and protect critical services.

---

## ⚙️ Configuration Summary
```bash
enable
configure terminal

! Configure Interfaces
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

! Access Control List
ip access-list extended HTTP-SERVER-ACCESS
 permit ip 192.168.10.0 0.0.0.255 host 192.168.10.5
 permit ip 192.168.20.0 0.0.0.255 host 192.168.10.5
 deny ip 192.168.30.0 0.0.0.255 host 192.168.10.5
 permit ip any any
exit

! Apply ACL to Interfaces
interface GigabitEthernet0/1
 ip access-group HTTP-SERVER-ACCESS in
exit
interface GigabitEthernet0/2
 ip access-group HTTP-SERVER-ACCESS in
exit

end
write memory


