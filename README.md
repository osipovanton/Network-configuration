# Network-configuration

# Architecture diagram
![gre](https://user-images.githubusercontent.com/79700810/131821357-b7786903-6ba2-4460-b712-b428e359e834.png)

![gre2-Page-2 drawio](https://user-images.githubusercontent.com/79700810/131821736-34571abd-b15b-4984-aee8-9464dfb0841f.png)

| R1            | R2            | R3|
| ------------- | ------------- | ------------- |
| en|1
conf t
hostname R1
interface gigabitethernet 0/0/0
no shutdown
ip address 212.12.12.1 255.255.255.248
exit
interface gigabitethernet 0/0/1
no shutdown
ip address 212.12.12.10 255.255.255.248
exit
interface loopback 1 
no shutdown
ip address 192.168.2.1 255.255.255.0
exit
interface Tunne 1
ip address 172.16.1.1 255.255.255.0
tunnel mode gre ip
tunnel source gigabitEthernet 0/0/0
tunnel destination 212.12.12.2
exit
interface Tunne 2
ip address 172.16.2.1 255.255.255.0
tunnel mode gre ip
tunnel source gigabitEthernet 0/0/1
tunnel destination 212.12.12.11
exit
ip route 192.168.1.0 255.255.255.0 172.16.1.2
ip route 192.168.3.0 255.255.255.0 172.16.2.2
exit
do copy running-config startup-config
exit
|2
