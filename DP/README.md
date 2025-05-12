# Проектная работа. 
## Построение гео-распределённой сети ЦОД на базе EVPN\VXLAN с использованием подхода MultiPod

### Схема сети

![схема.png](Схема.png)


### Цели:
- Обеспечение территориально распределенной сетевой инфраструктуры 
- Обеспечение географического резервирования вычислительных мощностей.
- Обеспечение возможности миграции серверов как внутри, так и между локациями, без необходимости переконфигурирования сетевого стека.
- Обеспечение горизонтального масштабирования сетевой инфраструктуры для подключения нового оборудования, без влияния на уже работающие сервисы
- Обеспечение отказоустойчивых подключений серверов критической инфраструктуры к оборудованию сетевой инфраструктуры

### Описание:
  #### В ходе работы мы:
  1. Соберем схему
  2. Распределим адресное пространство
  3. Разберемся с feature и включим необходимые
  4. Резберемся с TCAM и перераспределим для наших задач
  5. Настроим OSPF-Underlay
  6. Настроим VPC
  7. Настроим iBGP для Overlay
  8. Настроим L2 и L3 Vxlan

## Работа:
<details>
  <summary><b>Оборудование и технологии:</b></summary>
  <p>

- Eve-ng эмулятор
- Cisco Nexus 9k
- OSPF
- BGP
- EVPN\VxLan
- VPC
- SVI

  </p>
</details>

<details>
  <summary><b>Решения для построения отказоустойчивой и масштабируемой сетевой инфраструктуры</b></summary>
  <p>
    
1. CLOS топология дает возможности простого масштабирования ёмкости и производительности сети
2. Технология VXLAN Anycast Gateway позволяет обеспечивать миграцию подключений клиентов внутри сетевой инфраструктуры, с сохранением настроек сетевого стека
3. Имеется поддержка организации как L2 так и L3 сервисов. Ёмкость идентификатора VNI снимает ограничение по кол-ву клиентских сервисов
4. Технология VPC обеспечивает отказоустойчивое подключение клиентов
5. Технология DCI (Data Center Interconnect) обеспечивает бесшовное географическое распределение POD-ов
   	- Используется технология "multi-pod". Связь устанавливается через линки spine -- spine. Связь 3-3
  </p>
</details>

#### Адресное пространство:
<details>
  <summary><b>Spine</b></summary>
  <p>
     
![Spine.png](Spine.png)
  </p>
</details>
<details>
  <summary><b>Leaf</b></summary>
  <p>
     
![Leaf.png](Leaf.png)
  </p>
</details>
<details>
  <summary><b>PC</b></summary>
  <p>
     
![PC.png](PC.png)
  </p>
</details>

#### Feature:

<details>
  <summary><b>Spine</b></summary>
  <p>
    
```
nv overlay evpn
feature ospf
feature bgp
feature nv overlay
```
  </p>
</details>

<details>
  <summary><b>Leaf</b></summary>
  <p>

 ```
 nv overlay evpn
 feature ospf
 feature bgp
 feature fabric forwarding
 feature interface-vlan
 feature vn-segment-vlan-based
 feature lacp
 feature vpc
 feature nv overlay
 ```
 
  </p>
</details>

#### TCAM
<details>
  <summary><b>Leaf</b></summary>
  <p>
    
```
hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide
```
  </p>
</details>

#### OSPF

<details>
  <summary><b>Spine</b></summary>
  <p>
    
```
router ospf UNDERLAY
 router-id X.x.X.x
 passive-interface default

interface loopback1
ip address X.x.X.x/32
ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
ip address X.x.X.x/32
ip router ospf UNDERLAY area 0.0.0.0

interface EthernetX/X
ip address X.x.X.x/31
ip ospf network point-to-point
no ip ospf passive-interface
ip router ospf UNDERLAY area 0.0.0.0

```
  </p>
</details>

<details>
  <summary><b>Leaf</b></summary>
  <p>
    
```    
router ospf UNDERLAY
router-id X.x.X.x
passive-interface default

interface loopback1
ip address X.x.X.x/32
ip address X.x.X.x/32 sec
ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
ip address X.x.X.x/32
ip router ospf UNDERLAY area 0.0.0.0

interface EthernetX/X
ip address X.x.X.x/31
ip ospf network point-to-point
no ip ospf passive-interface
ip router ospf UNDERLAY area 0.0.0.0
	
 ```
  </p>
</details>

#### VPC
<details>
  <summary><b>Leaf</b></summary>
  <p>
    
```
vpc domain X - одинаковый на устройствах пары 
  peer-switch
  system-mac aa:11:ff:aa:11:11 - Уникальный для пары
  peer-keepalive destination 10.100.62.12  -  Через Management. Адрес соседа в паре.
  virtual peer-link destination 10.1.0.2 source 10.1.0.1 dscp 56 - Виртуальный через Lo2
  delay restore 60
  peer-gateway
  layer3 peer-router
  auto-recovery
  fast-convergence
  ip arp synchronize

interface port-channel1000
  switchport mode trunk
  spanning-tree port type network
  vpc peer-link

```
#### Порт для подключения устройства по VPC
```
interface Ethernet1/3 
  switchport mode trunk
  channel-group 13 mode active

interface port-channel13
  switchport mode trunk
  vpc 13

```
  </p>
</details>

#### iBGP 
<details>
  <summary><b>Spine</b></summary>
  <p>
    
```
route-map NEXTHOP permit 10
  set ip next-hop unchanged

router bgp 65500
  address-family l2vpn evpn
    maximum-paths 10
    maximum-paths ibgp 20
    nexthop route-map NEXTHOP
    advertise-pip
  template peer Peers
    remote-as 65500
    update-source loopback1
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor x.x.x.x  - Lo1 Спайнов и Лифов с кем пиримся
    inherit peer Peers
 

```
  </p>
</details>

<details>
  <summary><b>Leaf</b></summary>
  <p>
    
```
router bgp 65500
  address-family ipv4 unicast
    maximum-paths 10
  address-family l2vpn evpn
    maximum-paths 10
    advertise-pip
  template peer SPINES
    remote-as 65500
    update-source loopback1
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor x.x.x.x - Lo1 Спайнов
    inherit peer SPINES

```
  </p>
</details>

#### L2 и L3 Vxlan
<details>
  <summary><b>Leaf</b></summary>
  <p>
    
```
fabric forwarding anycast-gateway-mac 0000.0000.0001 

vlan 2
  name VxLan_L3
  vn-segment 5000
vlan 10
  name Vlan_10
  vn-segment 10010
vlan 20
  name Vlan_20
  vn-segment 10020
vlan 30
  name Vlan_30
  vn-segment 10030
vlan 40
  name Vlan_40
  vn-segment 10040

vrf context VXLAN
  vni 5000
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn


 interface Vlan2
  description VxLan_L3
  no shutdown
  vrf member VXLAN
  no ip redirects
  ip forward
  no ipv6 redirects

interface Vlan10
  no shutdown
  vrf member VXLAN
  no ip redirects
  ip address 192.168.10.1/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway

interface Vlan20
  no shutdown
  vrf member VXLAN
  no ip redirects
  ip address 192.168.20.1/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway

interface Vlan30
  no shutdown
  vrf member VXLAN
  no ip redirects
  ip address 192.168.30.1/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway

interface Vlan40
  no shutdown
  vrf member VXLAN
  no ip redirects
  ip address 192.168.40.1/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway


interface nve1
  no shutdown
  host-reachability protocol bgp
  advertise virtual-rmac
  source-interface loopback1
  global suppress-arp
  global ingress-replication protocol bgp
  member vni 5000 associate-vrf
  member vni 10010
  member vni 10020
  member vni 10030
  member vni 10040

```
  </p>
</details>

## Проверка

<details>
  <summary><b>Ping</b></summary>
  <p>
     
![Ping.png](Ping.png)
  </p>
</details>
