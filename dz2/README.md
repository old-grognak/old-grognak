### Underlay. OSPF

на основе схемы из DZ1 настраиваем ospf

```
10.0.1.0/32 – Spine 1 Lo1
10.0.2.0/32 – Spine 2 Lo1
10.0.0.1/32 – Leaf 1 Lo1
10.0.0.2/32 – Leaf 2 Lo1
10.0.0.3/32 – Leaf 3 Lo1
10.0.0.4/32 – Leaf 4 Lo1

10.2.1.0/31 – p2p Spine 1 → Leaf 1
10.2.1.2/31 – p2p Spine 1 → Leaf 2
10.2.1.4/31 – p2p Spine 1 → Leaf 3
10.2.1.6/31 – p2p Spine 1 → Leaf 4
10.2.2.0/31 – p2p Spine 2 → Leaf 1
10.2.2.2/31 – p2p Spine 2 → Leaf 2
10.2.2.4/31 – p2p Spine 2 → Leaf 3
10.2.2.6/31 – p2p Spine 2 → Leaf 4
```

![image](https://github.com/user-attachments/assets/13e09027-e350-4f63-845d-075e0cde1fbd)

примеры настроек 
<details>
  <summary>config spine1</summary>
  
  ```
spine1#show running-config 
! Command: show running-config
! device: spine1 (vEOS-lab, EOS-4.28.1F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname spine1
!
spanning-tree mode mstp
!
interface Ethernet1
   no switchport
   ip address 10.2.1.0/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet2
   no switchport
   ip address 10.2.1.2/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
  interface Ethernet3
   no switchport
   ip address 10.2.1.4/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet4
   no switchport
   ip address 10.2.1.6/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.1.0/32
   ip ospf area 0.0.0.0
!
interface Management1
!
ip routing
!
router ospf 100
   router-id 10.0.1.0
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet3
   no passive-interface Ethernet4
   max-lsa 12000
!
end

  ```
</details>

<details>
<summary>config leaf1</summary>
  
```
spine2#show running-config 
! Command: show running-config
! device: spine2 (vEOS-lab, EOS-4.28.1F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname spine2
!
spanning-tree mode mstp
!
interface Ethernet1
   no switchport
   ip address 10.2.2.0/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet2
   no switchport
   ip address 10.2.2.2/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   no switchport
   ip address 10.2.2.4/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet4
   no switchport
   ip address 10.2.2.6/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.2.0/32
   ip ospf area 0.0.0.0
!
interface Management1
!
ip routing
!
router ospf 100
   router-id 10.0.2.0
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet3
   no passive-interface Ethernet4
   max-lsa 12000
!
end
  
``` 
</details>

<details>
  <summary>spine1#sho ip route ospf</summary>

  ```
spine1#sho ip route ospf

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

 O        10.0.0.1/32 [110/20] via 10.2.1.1, Ethernet1
 O        10.0.0.2/32 [110/20] via 10.2.1.3, Ethernet2
 O        10.0.0.3/32 [110/20] via 10.2.1.5, Ethernet3
 O        10.0.0.4/32 [110/20] via 10.2.1.7, Ethernet4
 O        10.0.2.0/32 [110/30] via 10.2.1.1, Ethernet1
                               via 10.2.1.3, Ethernet2
                               via 10.2.1.5, Ethernet3
                               via 10.2.1.7, Ethernet4
 O        10.2.2.0/31 [110/20] via 10.2.1.1, Ethernet1
 O        10.2.2.2/31 [110/20] via 10.2.1.3, Ethernet2
 O        10.2.2.4/31 [110/20] via 10.2.1.5, Ethernet3
 O        10.2.2.6/31 [110/20] via 10.2.1.7, Ethernet4

   ``` 
</details>

<details>
  <summary>spine1#show ip ospf database</summary> 
  
  ``` 
spine1#show ip ospf database 

            OSPF Router with ID(10.0.1.0) (Instance ID 100) (VRF default)

                 Router Link States (Area 0.0.0.0)

Link ID         ADV Router      Age         Seq#         Checksum Link count
10.0.0.1        10.0.0.1        562         0x80000006   0xaca4   5
10.0.2.0        10.0.2.0        132         0x80000008   0x1d61   9
10.0.0.2        10.0.0.2        358         0x80000006   0xf74e   5
10.0.0.4        10.0.0.4        131         0x80000005   0x90a0   5
10.0.0.3        10.0.0.3        242         0x80000006   0x43f7   5
10.0.1.0        10.0.1.0        138         0x80000007   0x1e6c   9
   ```
</details>
проверка доступности
<details>
  <summary>leaf1#ping 10.x.x.x</summary>

  ```
leaf1#ping 10.0.0.2
PING 10.0.0.2 (10.0.0.2) 72(100) bytes of data.
80 bytes from 10.0.0.2: icmp_seq=1 ttl=63 time=5.93 ms
80 bytes from 10.0.0.2: icmp_seq=2 ttl=63 time=5.14 ms
80 bytes from 10.0.0.2: icmp_seq=3 ttl=63 time=4.54 ms
80 bytes from 10.0.0.2: icmp_seq=4 ttl=63 time=4.19 ms
80 bytes from 10.0.0.2: icmp_seq=5 ttl=63 time=4.25 ms

--- 10.0.0.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 25ms
rtt min/avg/max/mdev = 4.192/4.815/5.934/0.653 ms, ipg/ewma 6.375/5.335 ms

leaf1#ping 10.0.0.3
PING 10.0.0.3 (10.0.0.3) 72(100) bytes of data.
80 bytes from 10.0.0.3: icmp_seq=1 ttl=63 time=6.98 ms
80 bytes from 10.0.0.3: icmp_seq=2 ttl=63 time=5.22 ms
80 bytes from 10.0.0.3: icmp_seq=3 ttl=63 time=4.87 ms
80 bytes from 10.0.0.3: icmp_seq=4 ttl=63 time=5.04 ms
80 bytes from 10.0.0.3: icmp_seq=5 ttl=63 time=5.10 ms

--- 10.0.0.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 28ms
rtt min/avg/max/mdev = 4.878/5.446/6.981/0.779 ms, ipg/ewma 7.160/6.186 ms

leaf1#ping 10.0.0.4
PING 10.0.0.4 (10.0.0.4) 72(100) bytes of data.
80 bytes from 10.0.0.4: icmp_seq=1 ttl=63 time=6.40 ms
80 bytes from 10.0.0.4: icmp_seq=2 ttl=63 time=5.20 ms
80 bytes from 10.0.0.4: icmp_seq=3 ttl=63 time=4.85 ms
80 bytes from 10.0.0.4: icmp_seq=4 ttl=63 time=4.59 ms
80 bytes from 10.0.0.4: icmp_seq=5 ttl=63 time=4.64 ms

--- 10.0.0.4 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 28ms
rtt min/avg/max/mdev = 4.598/5.139/6.401/0.669 ms, ipg/ewma 7.016/5.735 ms

leaf1#ping 10.2.2.6
PING 10.2.2.6 (10.2.2.6) 72(100) bytes of data.
80 bytes from 10.2.2.6: icmp_seq=1 ttl=64 time=3.40 ms
80 bytes from 10.2.2.6: icmp_seq=2 ttl=64 time=2.53 ms
80 bytes from 10.2.2.6: icmp_seq=3 ttl=64 time=3.34 ms
80 bytes from 10.2.2.6: icmp_seq=4 ttl=64 time=2.33 ms
80 bytes from 10.2.2.6: icmp_seq=5 ttl=64 time=3.33 ms

--- 10.2.2.6 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 16ms
rtt min/avg/max/mdev = 2.332/2.987/3.406/0.464 ms, ipg/ewma 4.074/3.199 ms
   ```
</details>
