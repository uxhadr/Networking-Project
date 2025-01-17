# Cisco Packet Tracer University Network Project

This project involves designing and configuring a network for a university with two campuses: a main campus and one smaller campus located 20 miles apart.

## Network Overview
## Main Campus (3 Buildings):
- **Building A:** Admin Staff, HR, Finance, Business Departments
- **Building B:** Engineering, and Art Departments
- **Building C:** Student Labs,and IT Departments

## Smaller Campus:
- **Health and Science Building:** Staff offices and Student Labs on separate floors

## Cloud Integration:
- **Email Server:** Hosted externally for university-wide communication

## Project Objectives:
- Assign each department to its own separate IP network.
- Configure switches with appropriate VLANs and security settings.
- Use **RIPv2** for internal network routing and static routing for the external email server.


---

## Network Design and Configuration

### Step 1: Core Network Setup
- Deployed a core router for central connectivity.
- Added a **Layer 3 Switch (3650-24PS)** to manage devices across the main campus devices.
- Added **Layer 2 switches** in each department and connected a PC to each.
- Equipped all routers with serial interfaces for WAN links.
- Established **Serial DCE** connections to the main campus router, cloud router, and smaller campus router.
- Linked the **L3 switch** to the main campus router and cascaded connections to departmental **L2 switches**.
- Applied visual organizaion in Cisco Packet Tracer using rectangular borders to distinguish buildings in the cloud.

### Step 2: Smaller Campus Configuration
- Connected the smaller campus **L3 switch** to its router.
- Connected the L3 switch to each department's **L2 switch**.
- Created VLANs for each department, starting from **VLAN 10 (192.168.1.0/24)** up to **VLAN 100 (192.168.10.0/24)**.

### Step 3: Interface Activation
- Enabled interfaces on the main campus router:
  ```
  enable
  configure terminal
  interface gigabitEthernet 0/0
  no shutdown
  ```
- Enabled interfaces connected to the smaller campus router (**interface serail 0/1/1**) and the cloud router (**interface serial 0/1/0**).
- Configured clock rates on **DCE** interfaces:
  ```
  int s0/1/1
  clock rate 64000
  int s0/0/0
  clock rate 64000
  do wr
  ```

### Step 4: VLAN Configuration on L2 Switches
- Configured VLANs on all **L2 switches**:
  ```
  en
  configure terminal
  int range fastEthernet 0/1-24
  switchport mode access
  switchport access vlan 10
  do wr
  ```
- Repeated for all VLANs up to VLAN 100.
![image](https://github.com/user-attachments/assets/7996911e-4e4c-42b3-9a11-649d63cd473b)

### Step 5: L3 Switch Configuration
- Configured access ports on the **L3 switch**:
  ```
  int gigabitEthernet 1/0/10
  switchport mode access
  switchport access vlan 40
  exit
  ```
- Enabled trunking on the **L3 switch**:
  ```
  int g1/0/10
  switchport trunk encapsulation dot1q
  switchport mode trunk
  ```

### Step 6: Subinterface Configuration for VLANs
- Configured subinterfaces on the router to correspond with VLANs:
  ```
  int g0/0.10
  encapsulation dot1Q 10
  ip address 192.168.1.1 255.255.255.0
  exit
  ```
This configuration was systematically repeated for all VLANS upto VLAN 100 (`int g0/0.100` | IP address 192.168.10.1/24)

### Step 7: DHCP Configuration
- Enabled DHCP:
  ```
  service dhcp
  ```
- Created DHCP pools for each department to allocate IP addresses dynamically:
  ```
  ip dhcp pool admin-pool
  network 192.168.1.0 255.255.255.0
  default-router 192.168.1.1
  dns-server 192.168.1.1
  exit
  ```
- Verified DHCP functionality by confirming that devices automatically received IP addresses from their designated VLAN pools.
  ![image](https://github.com/user-attachments/assets/2318ba3c-36ca-4fae-b5e7-2f2720c06983)

**Assigned a static IP address** to the email server to ensure consistent accessibility and reliable communication across the network.
![image](https://github.com/user-attachments/assets/e797ab24-dcd9-491d-befc-2b05ab5c579e)

**Purpose:** Dynamic addressing simplifies device management, while the static IP for the email server ensures uninterrupted service availability.


### Step 8: Routing Configuration
While troubleshooting connectivity from a PC on the main campus to devices in the smaller campus network, I encountered an error message stating, "Destination Host Unreachable."
![image](https://github.com/user-attachments/assets/261c86ae-a3ed-4318-8ea4-c28acdf998b2)


To resolve this issue, I implemented the Routing Information Protocol version 2 **(RIPv2)** to enable inter-VLAN and campus-to-campus communication.

**Configuration on the smaller campus router:**
```
router rip
version 2
network 192.168.9.0
network 192.168.10.0
network 10.10.10.0
do wr
```

**Configuration on the main campus router:**
```
router rip
version 2
network 192.168.1.0
network 192.168.2.0
network 192.168.3.0
network 10.10.10.0
do wr
```
After applying the routing configuration, I tested the connectivity to verify that the email server in the cloud could successfully ping devices across all campuses. The test was successful. 
![Physical](https://github.com/user-attachments/assets/e3544800-5581-4131-abc3-e438c70123f3)

## Conclusion
This project successfully demonstrates the design and implementation of a scalable, secure, and efficient university network. Utilizing VLAN segmentation, dynamic routing with RIPv2, and DHCP for address management ensures secure departmental communication and reliable inter-campus connectivity.


----

## How to View the Network Design

1. **Download the Project File:**  
   Click [**here**](./university_network_project.pkt) to download the Cisco Packet Tracer file.

