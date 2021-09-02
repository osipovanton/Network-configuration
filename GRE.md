# Network-configuration

Ниже приведен пример как создать GRE туннель между трюмя точками, чтобы создать и проверить GRE туннель между трюмя сетями. Внутренние сети R1, R2, R3 (192.168.2.0/24, 192.168.1.0/24, 192.168.3.0/24 ) соединяются с помощью GRE туннеля через интернет.

# Architecture diagram
![gre](https://user-images.githubusercontent.com/79700810/131821357-b7786903-6ba2-4460-b712-b428e359e834.png)
--
![gre2-Page-2 drawio](https://user-images.githubusercontent.com/79700810/131821736-34571abd-b15b-4984-aee8-9464dfb0841f.png)
--


|R1             |R2             |R3             |
| ------------- | ------------- | ------------- |   
en |en   |en|
conf t|conf t   |conf t |
hostname R1|  hostname R2 |hostname R3 |
interface gigabitethernet 0/0/0| interface gigabitethernet 0/0/0  |interface gigabitethernet 0/0/0 |
no shutdown| no shutdown  |no shutdown |
ip address 212.12.12.1 255.255.255.248| ip address 212.12.12.2 255.255.255.248  | ip address 212.12.12.21 255.255.255.248|
exit|exit   |exit |
interface gigabitethernet 0/0/1|interface gigabitethernet 0/0/1   |interface gigabitethernet 0/0/1 |
no shutdown|  no shutdown |no shutdown |
ip address 212.12.12.10 255.255.255.248|ip address 212.12.12.20 255.255.255.248   | ip address 212.12.12.11 255.255.255.248|
exit|  exit |exit |
interface loopback 1 | interface loopback 1  |interface loopback 1  |
no shutdown| no shutdown  | no shutdown|
ip address 192.168.2.1 255.255.255.0| ip address 192.168.1.1 255.255.255.0  | ip address 192.168.3.1 255.255.255.0|
exit|  exit |exit |
---
|R1             |R2             |R3             |
| ------------- | ------------- | ------------- | 
interface Tunne 1| interface Tunne1  |interface Tunne2 |
ip address 172.16.1.1 255.255.255.0| ip address 172.16.1.2 255.255.255.0  | ip address 172.16.2.2 255.255.255.0|
tunnel mode gre ip| tunnel mode gre ip  |tunnel mode gre ip |
tunnel source gigabitEthernet 0/0/0|  tunnel source gigabitEthernet 0/0/0 | tunnel source gigabitEthernet 0/0/1|
tunnel destination 212.12.12.2| tunnel destination 212.12.12.1  |tunnel destination 212.12.12.10 |
exit|  exit |exit |
interface Tunne 2|  interface Tunne3 |interface Tunne3 |
ip address 172.16.2.1 255.255.255.0| ip address 172.16.3.1 255.255.255.0  |ip address 172.16.3.2 255.255.255.0 |
tunnel mode gre ip| tunnel mode gre ip  |tunnel mode gre ip |
tunnel source gigabitEthernet 0/0/1|  tunnel source gigabitEthernet 0/0/1 |tunnel source gigabitEthernet 0/0/0 |
tunnel destination 212.12.12.11| tunnel destination 212.12.12.21  | tunnel destination 212.12.12.20|
exit|  exit | exit|
ip route 192.168.1.0 255.255.255.0 172.16.1.2| ip route 192.168.2.0 255.255.255.0 172.16.1.1  |ip route 192.168.1.0 255.255.255.0 172.16.3.1 |
ip route 192.168.3.0 255.255.255.0 172.16.2.2| ip route 192.168.3.0 255.255.255.0 172.16.3.2  |ip route 192.168.2.0 255.255.255.0 172.16.2.1 |
exit| exit  |exit |
do copy running-config startup-config| do copy running-config startup-config  | do copy running-config startup-config|
exit| exit  |exit|
