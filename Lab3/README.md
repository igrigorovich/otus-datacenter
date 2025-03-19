# Домашние задание 3
## Построение Underlay сети(ISIS)

### Схема сети

![схема.png](схема.png)

# net выбран по принципу:
  # 49.0001.0000.0001.000X.00 - спайны
  # 49.0001.0000.0000.100X.00 - лифы

## Конфигурация и таблица маршрутизации

<details>
  <summary><b> Spine-1 </b></summary>
  <p> 

```
feature isis

interface Ethernet1/1
  description to leaf-1
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.1.0/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/2
  description to leaf-2
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.1.2/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/3
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.1.4/31
  ip router isis UNDERLAY
  no shutdown

interface loopback1
  ip address 10.0.1.0/32
  ip router isis UNDERLAY

interface loopback2
  ip address 10.1.1.0/32

router isis UNDERLAY
  net 49.0001.0000.0001.0001.00
  is-type level-1
  set-overload-bit on-startup 60
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 1/0
    *via 10.2.1.1, Eth1/1, [115/41], 00:01:46, isis-UNDERLAY, L1
10.0.0.2/32, ubest/mbest: 1/0
    *via 10.2.1.3, Eth1/2, [115/41], 00:01:46, isis-UNDERLAY, L1
10.0.0.3/32, ubest/mbest: 1/0
    *via 10.2.1.5, Eth1/3, [115/41], 00:01:46, isis-UNDERLAY, L1
10.0.1.0/32, ubest/mbest: 2/0, attached
    *via 10.0.1.0, Lo1, [0/0], 00:01:46, local
    *via 10.0.1.0, Lo1, [0/0], 00:01:46, direct
10.0.2.0/32, ubest/mbest: 3/0
    *via 10.2.1.1, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.1.3, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.1.5, Eth1/3, [115/81], 00:01:46, isis-UNDERLAY, L1
10.1.1.0/32, ubest/mbest: 2/0, attached
    *via 10.1.1.0, Lo2, [0/0], 00:01:46, local
    *via 10.1.1.0, Lo2, [0/0], 00:01:46, direct
10.2.1.0/31, ubest/mbest: 1/0, attached
    *via 10.2.1.0, Eth1/1, [0/0], 00:01:46, direct
10.2.1.0/32, ubest/mbest: 1/0, attached
    *via 10.2.1.0, Eth1/1, [0/0], 00:01:46, local
10.2.1.2/31, ubest/mbest: 1/0, attached
    *via 10.2.1.2, Eth1/2, [0/0], 00:01:46, direct
10.2.1.2/32, ubest/mbest: 1/0, attached
    *via 10.2.1.2, Eth1/2, [0/0], 00:01:46, local
10.2.1.4/31, ubest/mbest: 1/0, attached
    *via 10.2.1.4, Eth1/3, [0/0], 00:01:46, direct
10.2.1.4/32, ubest/mbest: 1/0, attached
    *via 10.2.1.4, Eth1/3, [0/0], 00:01:46, local
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.1.1, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.1.3, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.1.5, Eth1/3, [115/80], 00:01:46, isis-UNDERLAY, L1
```

</p>
</details>

<details>
  <summary><b> Spine-2 </b></summary>
  <p> 

```
feature isis

interface Ethernet1/1
  description to leaf-1
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.2.0/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/2
  description to leaf-2
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.2.2/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/3
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.2.4/31
  ip router isis UNDERLAY
  no shutdown

interface loopback1
  ip address 10.0.2.0/32
  ip router isis UNDERLAY

interface loopback2
  ip address 10.1.2.0/32

router isis UNDERLAY
  net 49.0001.0000.0001.0002.00
  is-type level-1
  set-overload-bit on-startup 60
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 1/0
    *via 10.2.2.1, Eth1/1, [115/41], 00:01:45, isis-UNDERLAY, L1
10.0.0.2/32, ubest/mbest: 1/0
    *via 10.2.2.3, Eth1/2, [115/41], 00:01:45, isis-UNDERLAY, L1
10.0.0.3/32, ubest/mbest: 1/0
    *via 10.2.2.5, Eth1/3, [115/41], 00:01:44, isis-UNDERLAY, L1
10.0.1.0/32, ubest/mbest: 3/0
    *via 10.2.2.1, Eth1/1, [115/81], 00:01:45, isis-UNDERLAY, L1
    *via 10.2.2.3, Eth1/2, [115/81], 00:01:44, isis-UNDERLAY, L1
    *via 10.2.2.5, Eth1/3, [115/81], 00:01:44, isis-UNDERLAY, L1
10.0.2.0/32, ubest/mbest: 2/0, attached
    *via 10.0.2.0, Lo1, [0/0], 00:01:46, local
    *via 10.0.2.0, Lo1, [0/0], 00:01:46, direct
10.1.2.0/32, ubest/mbest: 2/0, attached
    *via 10.1.2.0, Lo2, [0/0], 00:01:46, local
    *via 10.1.2.0, Lo2, [0/0], 00:01:46, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.2.1, Eth1/1, [115/80], 00:01:45, isis-UNDERLAY, L1
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.2.3, Eth1/2, [115/80], 00:01:45, isis-UNDERLAY, L1
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.2.5, Eth1/3, [115/80], 00:01:44, isis-UNDERLAY, L1
10.2.2.0/31, ubest/mbest: 1/0, attached
    *via 10.2.2.0, Eth1/1, [0/0], 00:01:46, direct
10.2.2.0/32, ubest/mbest: 1/0, attached
    *via 10.2.2.0, Eth1/1, [0/0], 00:01:46, local
10.2.2.2/31, ubest/mbest: 1/0, attached
    *via 10.2.2.2, Eth1/2, [0/0], 00:01:46, direct
10.2.2.2/32, ubest/mbest: 1/0, attached
    *via 10.2.2.2, Eth1/2, [0/0], 00:01:46, local
10.2.2.4/31, ubest/mbest: 1/0, attached
    *via 10.2.2.4, Eth1/3, [0/0], 00:01:46, direct
10.2.2.4/32, ubest/mbest: 1/0, attached
    *via 10.2.2.4, Eth1/3, [0/0], 00:01:46, local
```

</p>
</details>

<details>
  <summary><b> Leaf-1</b></summary>
  <p>
 
```
feature isis


interface Ethernet1/1
  description to Spine-1
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.1.1/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/2
  description to Spine-2
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.2.1/31
  ip router isis UNDERLAY
  no shutdown


interface loopback1
  ip address 10.0.0.1/32
  ip router isis UNDERLAY

interface loopback2
  ip address 10.1.0.1/32

router isis UNDERLAY
  net 49.0001.0000.0000.1001.00
  is-type level-1
  set-overload-bit on-startup 60
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 2/0, attached
    *via 10.0.0.1, Lo1, [0/0], 00:01:46, local
    *via 10.0.0.1, Lo1, [0/0], 00:01:46, direct
10.0.0.2/32, ubest/mbest: 2/0
    *via 10.2.1.0, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.2.0, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
10.0.0.3/32, ubest/mbest: 2/0
    *via 10.2.1.0, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.2.0, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
10.0.1.0/32, ubest/mbest: 1/0
    *via 10.2.1.0, Eth1/1, [115/41], 00:01:46, isis-UNDERLAY, L1
10.0.2.0/32, ubest/mbest: 1/0
    *via 10.2.2.0, Eth1/2, [115/41], 00:01:46, isis-UNDERLAY, L1
10.1.0.1/32, ubest/mbest: 2/0, attached
    *via 10.1.0.1, Lo2, [0/0], 00:01:46, local
    *via 10.1.0.1, Lo2, [0/0], 00:01:46, direct
10.2.1.0/31, ubest/mbest: 1/0, attached
    *via 10.2.1.1, Eth1/1, [0/0], 00:01:46, direct
10.2.1.1/32, ubest/mbest: 1/0, attached
    *via 10.2.1.1, Eth1/1, [0/0], 00:01:46, local
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.1.0, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.1.0, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.0/31, ubest/mbest: 1/0, attached
    *via 10.2.2.1, Eth1/2, [0/0], 00:01:46, direct
10.2.2.1/32, ubest/mbest: 1/0, attached
    *via 10.2.2.1, Eth1/2, [0/0], 00:01:46, local
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.2.0, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.2.0, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1

```  
  </p>
</details>

<details>
  <summary><b> Leaf-2</b></summary>
  <p>
 
```
feature isis

interface Ethernet1/1
  description to Spine-1
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.1.3/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/2
  description to Spine-2
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.2.3/31
  ip router isis UNDERLAY
  no shutdown

interface loopback1
  ip address 10.0.0.2/32
  ip router isis UNDERLAY

interface loopback2
  ip address 10.1.0.2/32

router isis UNDERLAY
  net 49.0001.0000.0000.1002.00
  is-type level-1
  set-overload-bit on-startup 60

```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 2/0
    *via 10.2.1.2, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.2.2, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
10.0.0.2/32, ubest/mbest: 2/0, attached
    *via 10.0.0.2, Lo1, [0/0], 00:01:46, local
    *via 10.0.0.2, Lo1, [0/0], 00:01:46, direct
10.0.0.3/32, ubest/mbest: 2/0
    *via 10.2.1.2, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.2.2, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
10.0.1.0/32, ubest/mbest: 1/0
    *via 10.2.1.2, Eth1/1, [115/41], 00:01:46, isis-UNDERLAY, L1
10.0.2.0/32, ubest/mbest: 1/0
    *via 10.2.2.2, Eth1/2, [115/41], 00:01:46, isis-UNDERLAY, L1
10.1.0.2/32, ubest/mbest: 2/0, attached
    *via 10.1.0.2, Lo2, [0/0], 00:01:46, local
    *via 10.1.0.2, Lo2, [0/0], 00:01:46, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.1.2, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.1.2/31, ubest/mbest: 1/0, attached
    *via 10.2.1.3, Eth1/1, [0/0], 00:01:46, direct
10.2.1.3/32, ubest/mbest: 1/0, attached
    *via 10.2.1.3, Eth1/1, [0/0], 00:01:46, local
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.1.2, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.2.2, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.2/31, ubest/mbest: 1/0, attached
    *via 10.2.2.3, Eth1/2, [0/0], 00:01:46, direct
10.2.2.3/32, ubest/mbest: 1/0, attached
    *via 10.2.2.3, Eth1/2, [0/0], 00:01:46, local
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.2.2, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1

```  
  </p>
</details>

<details>
  <summary><b> Leaf-3</b></summary>
  <p>
 
```
feature isis

interface Ethernet1/1
  description to Spine-1
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.1.5/31
  ip router isis UNDERLAY
  no shutdown

interface Ethernet1/2
  description to Spine-2
  no switchport
  mtu 9000
  no ip redirects
  ip address 10.2.2.5/31
  ip router isis UNDERLAY
  no shutdown

interface loopback1
  ip address 10.0.0.3/32
  ip router isis UNDERLAY

interface loopback2
  ip address 10.1.0.3/32

router isis UNDERLAY
  net 49.0001.0000.0000.1003.00
  is-type level-1
  set-overload-bit on-startup 60
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 2/0
    *via 10.2.1.4, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.2.4, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
10.0.0.2/32, ubest/mbest: 2/0
    *via 10.2.1.4, Eth1/1, [115/81], 00:01:46, isis-UNDERLAY, L1
    *via 10.2.2.4, Eth1/2, [115/81], 00:01:46, isis-UNDERLAY, L1
10.0.0.3/32, ubest/mbest: 2/0, attached
    *via 10.0.0.3, Lo1, [0/0], 00:01:46, local
    *via 10.0.0.3, Lo1, [0/0], 00:01:46, direct
10.0.1.0/32, ubest/mbest: 1/0
    *via 10.2.1.4, Eth1/1, [115/41], 00:01:46, isis-UNDERLAY, L1
10.0.2.0/32, ubest/mbest: 1/0
    *via 10.2.2.4, Eth1/2, [115/41], 00:01:46, isis-UNDERLAY, L1
10.1.0.3/32, ubest/mbest: 2/0, attached
    *via 10.1.0.3, Lo2, [0/0], 00:01:46, local
    *via 10.1.0.3, Lo2, [0/0], 00:01:46, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.1.4, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.1.4, Eth1/1, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.1.4/31, ubest/mbest: 1/0, attached
    *via 10.2.1.5, Eth1/1, [0/0], 00:01:46, direct
10.2.1.5/32, ubest/mbest: 1/0, attached
    *via 10.2.1.5, Eth1/1, [0/0], 00:01:46, local
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.2.4, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.2.4, Eth1/2, [115/80], 00:01:46, isis-UNDERLAY, L1
10.2.2.4/31, ubest/mbest: 1/0, attached
    *via 10.2.2.5, Eth1/2, [0/0], 00:01:46, direct
10.2.2.5/32, ubest/mbest: 1/0, attached
    *via 10.2.2.5, Eth1/2, [0/0], 00:01:46, local
```  
  </p>
</details>

