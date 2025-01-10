# Cisco Packet Tracer University Network Project

This project involves creating a network for a university with two campuses (one main and one smaller) located 20 miles apart.

## Main Campus (3 Buildings):
- **Building A:** Admin Staff, HR, Finance, Business
- **Building B:** Engineering, Art
- **Building C:** Student Labs, IT Department

## Smaller Campus:
- **Health and Science Building:** Staff offices and Student Labs on separate floors

## Cloud:
- **Email Server**

## Project Goals:
- Each department should be on its own separate IP network.
- Switches must be configured with appropriate VLANs and security settings.
- **RIPv2** will be used for internal network routing and static routing for the external email server.

---

## Network Design and Configuration

### Step 1: Core Network Setup
- Selected a router as the core of the network.
- Added a **Layer 3 Switch (3650-24PS)** to manage the main campus devices.
- Added a switch in each department and connected a PC to each.
- Installed serial interfaces on all routers.
- Connected the cloud and the smaller campus routers to the main campus router using **Serial DCE** connections.
- Connected the **L3 switch** to the main campus router and then to all departmental **L2 switches**.
- Used rectangular borders to visually separate each building and the cloud in Packet Tracer.

### Step 2: Smaller Campus Setup
- Connected the smaller campus **L3 switch** to its router.
- Connected the L3 switch to each department's **L2 switch**.
- Created VLANs for each department, starting from **VLAN 10 (192.168.1.0/24)** up to **VLAN 100 (192.168.10.0/24)**.

### Step 3: Interface Activation
- Enabled interfaces on the main campus router:
  ```
  en
  conf t
  int g0/0
  no shut
  ```
- Enabled interfaces connected to the smaller campus router (**int s0/1/1**) and the cloud router (**int s0/1/0**).
- Set clock rates on **DCE** interfaces:
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
  conf t
  int range f0/1-24
  switchport mode access
  switchport access vlan 10
  do wr
  ```
- Repeated for all VLANs up to VLAN 100.
![image](https://github.com/user-attachments/assets/7996911e-4e4c-42b3-9a11-649d63cd473b)

### Step 5: L3 Switch Configuration
- Configured access ports on the **L3 switch**:
  ```
  int g1/0/10
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
- Configured subinterfaces on the router to match VLANs:
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

**Purpose:** Dynamic addressing simplifies device management, while the static IP for the email server ensures uninterrupted service availability.

I assigned the email server a static IP address:
![image](https://github.com/user-attachments/assets/e797ab24-dcd9-491d-befc-2b05ab5c579e)

### Step 8: Routing Configuration
While troubleshooting connectivity from a PC on the main campus to devices in the smaller campus network, I encountered an error message stating, "Destination Host Unreachable."
![image](https://github.com/user-attachments/assets/261c86ae-a3ed-4318-8ea4-c28acdf998b2)


To resolve this issue, I implemented the Routing Information Protocol version 2 (RIPv2) to enable inter-VLAN and campus-to-campus communication.

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
This project effectively demonstrates how to design and implement a multi-campus university network using VLANs, routing protocols, and DHCP for dynamic IP addressing. The setup ensures secure, segmented communication between departments and reliable connectivity across campuses.

