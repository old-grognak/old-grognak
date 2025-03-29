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

