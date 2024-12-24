# Networking-Project

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

