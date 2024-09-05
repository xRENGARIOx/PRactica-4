
### PCA
```bash
ifconfig enp1s0 192.168.20.13 netmask 255.255.255.0 up
route add default gw 192.168.20.1
```

### PCB
```bash 
ifconfig enp0s25 192.168.30.13 netmask 255.255.255.0 up
route add default gw 192.168.30.1
```

### Router
```ciscoIOS
enable

conf t
hostname R1

no ip domain lookup

enable secret class

line console 0 
password cisco 
login

line vty 0 4 
password cisco 
login

service password-encryption

banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Practica - 4            |
 | 05 de semptiembre  2024 |
 | R1                      |
 | MAYOGI                  |
 | Acceso restringido !!!  |
[+]-----------------------[+]
$

!copy running-config startup-config
```
## Switch 1
```ciscoIOS
enable

conf t
hostname S1

no ip domain lookup

enable secret class

line console 0 
password cisco 
login

line vty 0 15 
password cisco 
login

service password-encryption

banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Practica - 4            |
 | 05 de semptiembre  2024 |
 | S1                      |
 | MAYOGI                  |
 | Acceso restringido !!!  |
[+]-----------------------[+]
$

!copy running-config startup-config

```


## Switch 2
```ciscoIOS
enable

conf t
hostname S2

no ip domain lookup

enable secret class

line console 0 
password cisco 
login

line vty 0 15 
password cisco 
login

service password-encryption

banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Practica - 4            |
 | 05 de semptiembre  2024 |
 | S2                      |
 | MAYOGI                  |
 | Acceso restringido !!!  |
[+]-----------------------[+]
$

!copy running-config startup-config
```



## SW1 VLANs

```
conf t 
vlan 10 
name Managment

vlan 20
name Sales

vlan 30
name Operations

vlan 999 
name Parking_Lot

vlan 1000
name Native

interface fa0/6
switchport mode acces
switchport acces vlan 20


interface range fa0/2 - 4
switchport mode acces
switchport acces vlan 999
shutdown

interface range fa0/7 - 24
switchport mode acces
switchport acces vlan 999
shutdown

interface range g0/1 - 2
switchport mode acces
switchport acces vlan 999
shutdown

interface vlan 10
ip address 192.168.10.11 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.20.11 255.255.255.0
no shutdown

interface vlan 30
ip address 192.168.30.11 255.255.255.0
no shutdown

```

### SW2 VLANs
```ciscoIOS
conf t 
vlan 10 
name Administration

vlan 30
name Operations

vlan 999 
name Parking_Lot

vlan 1000
name Native

! - Asignar el puerto 18 a la VLAN 30
interface fa0/18
switchport mode acces
switchport acces vlan 30


interface range fa0/2 - fa0/17
switchport mode acces
switchport acces vlan 999
shutdown

interface range fa0/19 - fa0/24
switchport mode acces
switchport acces vlan 999
shutdown

interface range g0/1 - g0/2
switchport mode acces
switchport acces vlan 999
shutdown

```


### Configurar puerto troncal en S1
```cicoIOS
conf t
interface fa0/1
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
end 
show interfaces trunk

```

### Configurar puerto troncal en S2
```
conf t
interface fa0/1
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
end 
show interfaces trunk
```

### Enlace Router a Switch
```ciscoIOS
conf t
interface fa0/5
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
end 
show interfaces trunk

```

fa0/5 no esta activo porque el g0/1 no esta activado

### Router
```ciscoIOS
enable
conf t
int g0/1.10
desc VLAN 10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
int g0/1.20
desc VLAN 20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
int g0/1.30
desc VLAN 30
encapsulation dot1q 30
ip address 192.168.30.1 255.255.255.0
int g0/1.1000
desc VLAN Native
encapsulation dot1q 1000 native

int g0/1 
no shutdown

end 
show ip interface brief



```
