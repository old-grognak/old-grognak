### Underlay. IS-IS

на основе схемы из DZ1 настраиваем IS-IS для Underlay сети

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

<details>
  <summary>config leaf1</summary>

```
leaf1#show running-config
! Command: show running-config
! device: leaf1 (vEOS-lab, EOS-4.28.1F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname leaf1
!
spanning-tree mode mstp
!
interface Ethernet1
   no switchport
   ip address 10.2.1.1/31
   isis enable 200
   isis bfd
   isis network point-to-point
!
interface Ethernet2
   no switchport
   ip address 10.2.2.1/31
   isis enable 200
   isis bfd
   isis network point-to-point
!
interface Ethernet3
   shutdown
!
interface Ethernet4
   shutdown
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
   ip address 10.0.0.1/32
   isis enable 200
!
interface Management1
!
ip routing
!
router isis 200
   net 49.0010.0000.0000.0001.00
   router-id ipv4 10.0.0.1
   is-type level-1
   !
   address-family ipv4 unicast
!
end

```
</details>

<details>
  <summary>show commands</summary>

```
leaf1#show ip route isis

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

 I L1     10.0.0.2/32 [115/30] via 10.2.1.0, Ethernet1
                               via 10.2.2.0, Ethernet2
 I L1     10.0.0.3/32 [115/30] via 10.2.1.0, Ethernet1
                               via 10.2.2.0, Ethernet2
 I L1     10.0.0.4/32 [115/30] via 10.2.1.0, Ethernet1
                               via 10.2.2.0, Ethernet2
 I L1     10.0.1.0/32 [115/20] via 10.2.1.0, Ethernet1
 I L1     10.0.2.0/32 [115/20] via 10.2.2.0, Ethernet2
 I L1     10.2.1.2/31 [115/20] via 10.2.1.0, Ethernet1
 I L1     10.2.1.4/31 [115/20] via 10.2.1.0, Ethernet1
 I L1     10.2.1.6/31 [115/20] via 10.2.1.0, Ethernet1
 I L1     10.2.2.2/31 [115/20] via 10.2.2.0, Ethernet2
 I L1     10.2.2.4/31 [115/20] via 10.2.2.0, Ethernet2
 I L1     10.2.2.6/31 [115/20] via 10.2.2.0, Ethernet2

leaf1#show isis database

IS-IS Instance: 200 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  53936   717    121 L1 <>
    leaf2.00-00                   6  45518   825    121 L1 <>
    leaf3.00-00                   5  11336  1090    121 L1 <>
    leaf4.00-00                   3  22290   687    121 L1 <>
    spine1.00-00                 20  25262  1088    170 L1 <>
    spine2.00-00                 20  54829  1090    170 L1 <>
leaf1#show isis database level-1 detail

IS-IS Instance: 200 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  53936   703    121 L1 <>
      LSP generation remaining wait time: 0 ms
      Time remaining until refresh: 403 s
      NLPID: 0xCC(IPv4)
      Hostname: leaf1
      Area addresses: 49.0010
      Interface address: 10.0.0.1
      Interface address: 10.2.2.1
      Interface address: 10.2.1.1
      IS Neighbor          : spine1.00           Metric: 10
      IS Neighbor          : spine2.00           Metric: 10
      Reachability         : 10.0.0.1/32 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.0/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.0/31 Metric: 10 Type: 1 Up
      Router Capabilities: Router Id: 10.0.0.1 Flags: []
        Area leader priority: 250 algorithm: 0
    leaf2.00-00                   6  45518   811    121 L1 <>
      Remaining lifetime received: 1199 s Modified to: 1200 s
      NLPID: 0xCC(IPv4)
      Hostname: leaf2
      Area addresses: 49.0010
      Interface address: 10.2.2.3
      Interface address: 10.2.1.3
      Interface address: 10.0.0.2
      IS Neighbor          : spine2.00           Metric: 10
      IS Neighbor          : spine1.00           Metric: 10
      Reachability         : 10.2.2.2/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.2/31 Metric: 10 Type: 1 Up
      Reachability         : 10.0.0.2/32 Metric: 10 Type: 1 Up
      Router Capabilities: Router Id: 10.0.0.2 Flags: []
        Area leader priority: 250 algorithm: 0
    leaf3.00-00                   5  11336  1076    121 L1 <>
      Remaining lifetime received: 1199 s Modified to: 1200 s
      NLPID: 0xCC(IPv4)
      Hostname: leaf3
      Area addresses: 49.0010
      Interface address: 10.0.0.3
      Interface address: 10.2.2.5
      Interface address: 10.2.1.5
      IS Neighbor          : spine2.00           Metric: 10
      IS Neighbor          : spine1.00           Metric: 10
      Reachability         : 10.0.0.3/32 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.4/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.4/31 Metric: 10 Type: 1 Up
      Router Capabilities: Router Id: 10.0.0.3 Flags: []
        Area leader priority: 250 algorithm: 0
    leaf4.00-00                   3  22290   673    121 L1 <>
      Remaining lifetime received: 1199 s Modified to: 1200 s
      NLPID: 0xCC(IPv4)
      Hostname: leaf4
      Area addresses: 49.0010
      Interface address: 10.0.0.4
      Interface address: 10.2.2.7
      Interface address: 10.2.1.7
      IS Neighbor          : spine2.00           Metric: 10
      IS Neighbor          : spine1.00           Metric: 10
      Reachability         : 10.0.0.4/32 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.6/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.6/31 Metric: 10 Type: 1 Up
      Router Capabilities: Router Id: 10.0.0.4 Flags: []
        Area leader priority: 250 algorithm: 0
    spine1.00-00                 20  25262  1074    170 L1 <>
      Remaining lifetime received: 1199 s Modified to: 1200 s
      NLPID: 0xCC(IPv4)
      Hostname: spine1
      Area addresses: 49.0010
      Interface address: 10.0.1.0
      Interface address: 10.2.1.6
      Interface address: 10.2.1.4
      Interface address: 10.2.1.2
      Interface address: 10.2.1.0
      IS Neighbor          : leaf3.00            Metric: 10
      IS Neighbor          : leaf4.00            Metric: 10
      IS Neighbor          : leaf2.00            Metric: 10
      IS Neighbor          : leaf1.00            Metric: 10
      Reachability         : 10.0.1.0/32 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.6/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.4/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.2/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.1.0/31 Metric: 10 Type: 1 Up
      Router Capabilities: Router Id: 10.0.1.0 Flags: []
        Area leader priority: 250 algorithm: 0
    spine2.00-00                 20  54829  1076    170 L1 <>
      Remaining lifetime received: 1199 s Modified to: 1200 s
      NLPID: 0xCC(IPv4)
      Hostname: spine2
      Area addresses: 49.0010
      Interface address: 10.0.2.0
      Interface address: 10.2.2.6
      Interface address: 10.2.2.4
      Interface address: 10.2.2.2
      Interface address: 10.2.2.0
      IS Neighbor          : leaf3.00            Metric: 10
      IS Neighbor          : leaf4.00            Metric: 10
      IS Neighbor          : leaf2.00            Metric: 10
      IS Neighbor          : leaf1.00            Metric: 10
      Reachability         : 10.0.2.0/32 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.6/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.4/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.2/31 Metric: 10 Type: 1 Up
      Reachability         : 10.2.2.0/31 Metric: 10 Type: 1 Up
      Router Capabilities: Router Id: 10.0.2.0 Flags: []
        Area leader priority: 250 algorithm: 0
leaf1#show isis neighbors

Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id
200       default  spine1           L1   Ethernet1          P2P               UP    24          0B
200       default  spine2           L1   Ethernet2          P2P               UP    29          0B

leaf1#ping 10.0.1.0
PING 10.0.1.0 (10.0.1.0) 72(100) bytes of data.
80 bytes from 10.0.1.0: icmp_seq=1 ttl=64 time=3.33 ms
80 bytes from 10.0.1.0: icmp_seq=2 ttl=64 time=2.44 ms
80 bytes from 10.0.1.0: icmp_seq=3 ttl=64 time=3.33 ms
80 bytes from 10.0.1.0: icmp_seq=4 ttl=64 time=2.26 ms
80 bytes from 10.0.1.0: icmp_seq=5 ttl=64 time=3.37 ms

--- 10.0.1.0 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 16ms
rtt min/avg/max/mdev = 2.261/2.950/3.374/0.493 ms, ipg/ewma 4.086/3.147 ms
leaf1#ping 10.0.2.0
PING 10.0.2.0 (10.0.2.0) 72(100) bytes of data.
80 bytes from 10.0.2.0: icmp_seq=1 ttl=64 time=3.07 ms
80 bytes from 10.0.2.0: icmp_seq=2 ttl=64 time=2.07 ms
80 bytes from 10.0.2.0: icmp_seq=3 ttl=64 time=2.02 ms
80 bytes from 10.0.2.0: icmp_seq=4 ttl=64 time=2.41 ms
80 bytes from 10.0.2.0: icmp_seq=5 ttl=64 time=2.31 ms

--- 10.0.2.0 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 18ms
rtt min/avg/max/mdev = 2.026/2.381/3.071/0.378 ms, ipg/ewma 4.724/2.722 ms
leaf1#ping 10.0.0.4
PING 10.0.0.4 (10.0.0.4) 72(100) bytes of data.
80 bytes from 10.0.0.4: icmp_seq=1 ttl=63 time=13.7 ms
80 bytes from 10.0.0.4: icmp_seq=2 ttl=63 time=5.52 ms
80 bytes from 10.0.0.4: icmp_seq=3 ttl=63 time=4.43 ms
80 bytes from 10.0.0.4: icmp_seq=4 ttl=63 time=4.36 ms
80 bytes from 10.0.0.4: icmp_seq=5 ttl=63 time=4.30 ms

--- 10.0.0.4 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 47ms
rtt min/avg/max/mdev = 4.302/6.467/13.706/3.648 ms, pipe 2, ipg/ewma 11.844/9.937 ms
leaf1#ping 10.2.2.5
PING 10.2.2.5 (10.2.2.5) 72(100) bytes of data.
80 bytes from 10.2.2.5: icmp_seq=1 ttl=63 time=6.30 ms
80 bytes from 10.2.2.5: icmp_seq=2 ttl=63 time=4.09 ms
80 bytes from 10.2.2.5: icmp_seq=3 ttl=63 time=4.32 ms
80 bytes from 10.2.2.5: icmp_seq=4 ttl=63 time=4.60 ms
80 bytes from 10.2.2.5: icmp_seq=5 ttl=63 time=5.66 ms

--- 10.2.2.5 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 27ms
rtt min/avg/max/mdev = 4.096/4.999/6.302/0.847 ms, ipg/ewma 6.883/5.663 ms
```
</details>
