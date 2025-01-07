# Cisco Packet Tracer University Network Project
This projectn involves creating a network for a university that has 2 campuses(one main and one small) that are 20 miles apart.

# Main campus has 3 buildings:
Building A: Admin staff , HR, Finance, Business
Building B: Engineering, Art 
Building C: Student labs, IT department

# Smaller Campus:
Health and Science Building which contains the Staff and Studnet Labs on seperate floors

# On the cloud:
Email Server

# Goals of the project: 
Each department is expected to be on its own seperate IP network
The switches should be configured with appropriate VLANs and securitys settings
RIPv2 will be used to provide routing for the routers in the internal network and static routing for the external server

I started by selecting a router which will be the core of the network. Next I added a L3 switch(3650-24PS) which will manage the devices in the main campus. Next I added a switch in each department and named them based on the department, I also added a PC in each department.

I added a serial interface on all the routers, I then connected the cloud and the smaller campus routers using serial DCE connections to the main campus router.I then connected the the the L3 switch to the main campus router and then connected the L3 switch to all the L2 switches in each department.
Next I seperated each building and the cloud using the rectangular borders, and named each building and department.

Next, I worked on the smaller campus, I connected its L3 switch to the smaller campus's router and then connected the L3 switch to each departments L2 switch.
I created vlans for each department, starting from VLAN 10 (192.168.1.0/24) through VLAN 100 (192.168.10.0/24)  
![image](https://github.com/user-attachments/assets/7996911e-4e4c-42b3-9a11-649d63cd473b)

I turned on interface on the main campus router that was connected to the L3 switch using the command:
```
en
conf t
int g0/0
no shut
```
I also turned on the interfaces connected to the smaller campus router(int s0/1/1) and the cloud router(int s0/1/0). 

I set a clock rate on the main campus router interfaces.
```
int s/0/1/1
clock rate 64000
int s0/0/0
clock rate 64000
do wr
```

I configured all the L2 switches with vlans:
```
en
conf t
int range f0/1-24
switchport mode ac
switchport ac vlan 10
do wr
```
I repeated the commands above for all the switches through vlan 100














Need to update up here casue I forgot to save my progress 
I set the clock rate for the DCE connections to the Main campus router to 64000.
I configured the vlans on all the L2 switches for each department using the following commands:

````
en
conf t

int range f0/1-24
switchport mode access
switchport access vlan 10

do wr
````

I configured the L3 switches ports so that the interfaces on the L3 swith are on the same vlans as the L2 switches.

````
en
conf t

int f0/5
switchport mode access
switchport access vlan 40

exit
``````

Next I configured trunking on the L3 switches so that traffic from all the vlans is able to pass through.
I entered the following commands in configuration mode:
```
int f0/3
switchport trunk encapsulation dot1q
switchport mode trunk
```

Next I assigned IP addresses to the interfaces of the routers connected to the main campus router.
I entered the following commands in configuration mode:
```
int s0/1/1
ip address 10.10.10.1 255.255.255.252
```

I assigned the Email server a static IP address.
![image](https://github.com/user-attachments/assets/63560256-ccb6-4ae8-bb21-6652bc137843)


Next I configred sub interfaces to match the Vlans 
I entered the following commands in configuration mode:
```
int g0/0.100
encapsulation dot1q 100
ip add 192.168.10.1 255.255.255.0

ex
do wr
```

I enabled dhcp with the command `service dhcp`
I created dhcp pools for each department with the command:
```
ip dhcp pool staff-pool
network 192.168.9.0 255.255.255.0
```

I used the address that I used to create the sub interfaces as the default router and dns server.
```
default-router 192.168.9.1
dns-server 192.168.9.1
```

I clicked on one of the PCs in the Vlan to see if it got an automatic ip address to see if the dhcp configuration worked:
![image](https://github.com/user-attachments/assets/7984d65f-3461-4612-8cc5-a2134d894335)

