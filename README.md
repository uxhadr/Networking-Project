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



