# Домашние задание 2
## Построение Underlay сети(OSPF)

### Схема сети

![схема.png](схема.png)

## Конфигурация и таблица маршрутизации

<details>
  <summary><b> Spine-1 </b></summary>
  <p> 

```
feature ospf

interface Ethernet1/1
  description to leaf-1
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.1.0/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to leaf-2
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.1.2/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.1.4/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.1.0/32
  ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
  ip address 10.1.1.0/32

router ospf UNDERLAY
  router-id 10.0.1.0
  passive-interface default
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 1/0
    *via 10.2.1.1, Eth1/1, [110/41], 01:24:04, ospf-UNDERLAY, intra
10.0.0.2/32, ubest/mbest: 1/0
    *via 10.2.1.3, Eth1/2, [110/41], 01:24:03, ospf-UNDERLAY, intra
10.0.0.3/32, ubest/mbest: 1/0
    *via 10.2.1.5, Eth1/3, [110/41], 01:19:37, ospf-UNDERLAY, intra
10.0.1.0/32, ubest/mbest: 2/0, attached
    *via 10.0.1.0, Lo1, [0/0], 01:52:43, local
    *via 10.0.1.0, Lo1, [0/0], 01:52:43, direct
10.0.2.0/32, ubest/mbest: 3/0
    *via 10.2.1.1, Eth1/1, [110/81], 01:24:03, ospf-UNDERLAY, intra
    *via 10.2.1.3, Eth1/2, [110/81], 01:23:57, ospf-UNDERLAY, intra
    *via 10.2.1.5, Eth1/3, [110/81], 01:19:37, ospf-UNDERLAY, intra
10.1.1.0/32, ubest/mbest: 2/0, attached
    *via 10.1.1.0, Lo2, [0/0], 01:52:43, local
    *via 10.1.1.0, Lo2, [0/0], 01:52:43, direct
10.2.1.0/31, ubest/mbest: 1/0, attached
    *via 10.2.1.0, Eth1/1, [0/0], 01:52:44, direct
10.2.1.0/32, ubest/mbest: 1/0, attached
    *via 10.2.1.0, Eth1/1, [0/0], 01:52:44, local
10.2.1.2/31, ubest/mbest: 1/0, attached
    *via 10.2.1.2, Eth1/2, [0/0], 01:52:44, direct
10.2.1.2/32, ubest/mbest: 1/0, attached
    *via 10.2.1.2, Eth1/2, [0/0], 01:52:44, local
10.2.1.4/31, ubest/mbest: 1/0, attached
    *via 10.2.1.4, Eth1/3, [0/0], 01:52:44, direct
10.2.1.4/32, ubest/mbest: 1/0, attached
    *via 10.2.1.4, Eth1/3, [0/0], 01:52:44, local
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.1.1, Eth1/1, [110/80], 01:24:04, ospf-UNDERLAY, intra
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.1.3, Eth1/2, [110/80], 01:24:03, ospf-UNDERLAY, intra
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.1.5, Eth1/3, [110/80], 01:19:37, ospf-UNDERLAY, intra
```

</p>
</details>

<details>
  <summary><b> Spine-2 </b></summary>
  <p> 

```
feature ospf

interface Ethernet1/1
  description to leaf-1
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.2.0/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to leaf-2
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.2.2/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.2.4/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.2.0/32
  ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
  ip address 10.1.2.0/32

router ospf UNDERLAY
  router-id 10.0.2.0
  passive-interface default
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 1/0
    *via 10.2.2.1, Eth1/1, [110/41], 01:29:27, ospf-UNDERLAY, intra
10.0.0.2/32, ubest/mbest: 1/0
    *via 10.2.2.3, Eth1/2, [110/41], 01:29:21, ospf-UNDERLAY, intra
10.0.0.3/32, ubest/mbest: 1/0
    *via 10.2.2.5, Eth1/3, [110/41], 01:25:00, ospf-UNDERLAY, intra
10.0.1.0/32, ubest/mbest: 3/0
    *via 10.2.2.1, Eth1/1, [110/81], 01:29:27, ospf-UNDERLAY, intra
    *via 10.2.2.3, Eth1/2, [110/81], 01:29:21, ospf-UNDERLAY, intra
    *via 10.2.2.5, Eth1/3, [110/81], 01:25:00, ospf-UNDERLAY, intra
10.0.2.0/32, ubest/mbest: 2/0, attached
    *via 10.0.2.0, Lo1, [0/0], 01:57:59, local
    *via 10.0.2.0, Lo1, [0/0], 01:57:59, direct
10.1.2.0/32, ubest/mbest: 2/0, attached
    *via 10.1.2.0, Lo2, [0/0], 01:57:59, local
    *via 10.1.2.0, Lo2, [0/0], 01:57:59, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.2.1, Eth1/1, [110/80], 01:29:27, ospf-UNDERLAY, intra
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.2.3, Eth1/2, [110/80], 01:29:21, ospf-UNDERLAY, intra
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.2.5, Eth1/3, [110/80], 01:25:00, ospf-UNDERLAY, intra
10.2.2.0/31, ubest/mbest: 1/0, attached
    *via 10.2.2.0, Eth1/1, [0/0], 01:58:01, direct
10.2.2.0/32, ubest/mbest: 1/0, attached
    *via 10.2.2.0, Eth1/1, [0/0], 01:58:01, local
10.2.2.2/31, ubest/mbest: 1/0, attached
    *via 10.2.2.2, Eth1/2, [0/0], 01:58:01, direct
10.2.2.2/32, ubest/mbest: 1/0, attached
    *via 10.2.2.2, Eth1/2, [0/0], 01:58:01, local
10.2.2.4/31, ubest/mbest: 1/0, attached
    *via 10.2.2.4, Eth1/3, [0/0], 01:58:00, direct
10.2.2.4/32, ubest/mbest: 1/0, attached
    *via 10.2.2.4, Eth1/3, [0/0], 01:58:00, local
```

</p>
</details>

<details>
  <summary><b> Leaf-1</b></summary>
  <p>
 
```
feature ospf

interface Ethernet1/1
  description to Spine-1
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.1.1/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to Spine-2
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.2.1/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.0.1/32
  ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
  ip address 10.1.0.1/32

router ospf UNDERLAY
  router-id 10.0.0.1
  passive-interface default
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 2/0, attached
    *via 10.0.0.1, Lo1, [0/0], 02:31:29, local
    *via 10.0.0.1, Lo1, [0/0], 02:31:29, direct
10.0.0.2/32, ubest/mbest: 2/0
    *via 10.2.1.0, Eth1/1, [110/81], 02:02:53, ospf-UNDERLAY, intra
    *via 10.2.2.0, Eth1/2, [110/81], 02:02:47, ospf-UNDERLAY, intra
10.0.0.3/32, ubest/mbest: 2/0
    *via 10.2.1.0, Eth1/1, [110/81], 01:58:27, ospf-UNDERLAY, intra
    *via 10.2.2.0, Eth1/2, [110/81], 01:58:27, ospf-UNDERLAY, intra
10.0.1.0/32, ubest/mbest: 1/0
    *via 10.2.1.0, Eth1/1, [110/41], 02:02:54, ospf-UNDERLAY, intra
10.0.2.0/32, ubest/mbest: 1/0
    *via 10.2.2.0, Eth1/2, [110/41], 02:02:53, ospf-UNDERLAY, intra
10.1.0.1/32, ubest/mbest: 2/0, attached
    *via 10.1.0.1, Lo2, [0/0], 02:31:29, local
    *via 10.1.0.1, Lo2, [0/0], 02:31:29, direct
10.2.1.0/31, ubest/mbest: 1/0, attached
    *via 10.2.1.1, Eth1/1, [0/0], 02:31:31, direct
10.2.1.1/32, ubest/mbest: 1/0, attached
    *via 10.2.1.1, Eth1/1, [0/0], 02:31:31, local
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.1.0, Eth1/1, [110/80], 02:02:54, ospf-UNDERLAY, intra
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.1.0, Eth1/1, [110/80], 01:58:38, ospf-UNDERLAY, intra
10.2.2.0/31, ubest/mbest: 1/0, attached
    *via 10.2.2.1, Eth1/2, [0/0], 02:31:30, direct
10.2.2.1/32, ubest/mbest: 1/0, attached
    *via 10.2.2.1, Eth1/2, [0/0], 02:31:30, local
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.2.0, Eth1/2, [110/80], 02:02:53, ospf-UNDERLAY, intra
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.2.0, Eth1/2, [110/80], 01:58:38, ospf-UNDERLAY, intra

```  
  </p>
</details>

<details>
  <summary><b> Leaf-2</b></summary>
  <p>
 
```
feature ospf

interface Ethernet1/1
  description to Spine-1
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.1.3/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to Spine-2
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.2.3/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.0.2/32
  ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
  ip address 10.1.0.2/32

router ospf UNDERLAY
  router-id 10.0.0.2
  passive-interface default
```
### Вывод маршрутной информации
```

10.0.0.1/32, ubest/mbest: 2/0
    *via 10.2.1.2, Eth1/1, [110/81], 02:09:03, ospf-UNDERLAY, intra
    *via 10.2.2.2, Eth1/2, [110/81], 02:08:56, ospf-UNDERLAY, intra
10.0.0.2/32, ubest/mbest: 2/0, attached
    *via 10.0.0.2, Lo1, [0/0], 02:37:38, local
    *via 10.0.0.2, Lo1, [0/0], 02:37:38, direct
10.0.0.3/32, ubest/mbest: 2/0
    *via 10.2.1.2, Eth1/1, [110/81], 02:04:36, ospf-UNDERLAY, intra
    *via 10.2.2.2, Eth1/2, [110/81], 02:04:36, ospf-UNDERLAY, intra
10.0.1.0/32, ubest/mbest: 1/0
    *via 10.2.1.2, Eth1/1, [110/41], 02:09:03, ospf-UNDERLAY, intra
10.0.2.0/32, ubest/mbest: 1/0
    *via 10.2.2.2, Eth1/2, [110/41], 02:08:56, ospf-UNDERLAY, intra
10.1.0.2/32, ubest/mbest: 2/0, attached
    *via 10.1.0.2, Lo2, [0/0], 02:37:38, local
    *via 10.1.0.2, Lo2, [0/0], 02:37:38, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.1.2, Eth1/1, [110/80], 02:09:03, ospf-UNDERLAY, intra
10.2.1.2/31, ubest/mbest: 1/0, attached
    *via 10.2.1.3, Eth1/1, [0/0], 02:37:40, direct
10.2.1.3/32, ubest/mbest: 1/0, attached
    *via 10.2.1.3, Eth1/1, [0/0], 02:37:40, local
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.1.2, Eth1/1, [110/80], 02:04:46, ospf-UNDERLAY, intra
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.2.2, Eth1/2, [110/80], 02:08:56, ospf-UNDERLAY, intra
10.2.2.2/31, ubest/mbest: 1/0, attached
    *via 10.2.2.3, Eth1/2, [0/0], 02:37:40, direct
10.2.2.3/32, ubest/mbest: 1/0, attached
    *via 10.2.2.3, Eth1/2, [0/0], 02:37:40, local
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.2.2, Eth1/2, [110/80], 02:04:46, ospf-UNDERLAY, intra
```  
  </p>
</details>

<details>
  <summary><b> Leaf-3</b></summary>
  <p>
 
```
feature ospf

interface Ethernet1/1
  description to Spine-1
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.1.5/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to Spine-2
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.2.2.5/31
  ip ospf network point-to-point
  ip router ospf UNDERLAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.0.3/32
  ip router ospf UNDERLAY area 0.0.0.0

interface loopback2
  ip address 10.1.0.3/32

router ospf UNDERLAY
  router-id 10.0.0.3
  passive-interface default
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 2/0
    *via 10.2.1.4, Eth1/1, [110/81], 02:05:55, ospf-UNDERLAY, intra
    *via 10.2.2.4, Eth1/2, [110/81], 02:05:49, ospf-UNDERLAY, intra
10.0.0.2/32, ubest/mbest: 2/0
    *via 10.2.1.4, Eth1/1, [110/81], 02:05:55, ospf-UNDERLAY, intra
    *via 10.2.2.4, Eth1/2, [110/81], 02:05:49, ospf-UNDERLAY, intra
10.0.0.3/32, ubest/mbest: 2/0, attached
    *via 10.0.0.3, Lo1, [0/0], 02:38:54, local
    *via 10.0.0.3, Lo1, [0/0], 02:38:54, direct
10.0.1.0/32, ubest/mbest: 1/0
    *via 10.2.1.4, Eth1/1, [110/41], 02:05:55, ospf-UNDERLAY, intra
10.0.2.0/32, ubest/mbest: 1/0
    *via 10.2.2.4, Eth1/2, [110/41], 02:05:49, ospf-UNDERLAY, intra
10.1.0.3/32, ubest/mbest: 2/0, attached
    *via 10.1.0.3, Lo2, [0/0], 02:38:54, local
    *via 10.1.0.3, Lo2, [0/0], 02:38:54, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.1.4, Eth1/1, [110/80], 02:05:55, ospf-UNDERLAY, intra
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.1.4, Eth1/1, [110/80], 02:05:55, ospf-UNDERLAY, intra
10.2.1.4/31, ubest/mbest: 1/0, attached
    *via 10.2.1.5, Eth1/1, [0/0], 02:38:55, direct
10.2.1.5/32, ubest/mbest: 1/0, attached
    *via 10.2.1.5, Eth1/1, [0/0], 02:38:55, local
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.2.4, Eth1/2, [110/80], 02:05:49, ospf-UNDERLAY, intra
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.2.4, Eth1/2, [110/80], 02:05:49, ospf-UNDERLAY, intra
10.2.2.4/31, ubest/mbest: 1/0, attached
    *via 10.2.2.5, Eth1/2, [0/0], 02:38:55, direct
10.2.2.5/32, ubest/mbest: 1/0, attached
    *via 10.2.2.5, Eth1/2, [0/0], 02:38:55, local

```  
  </p>
</details>

