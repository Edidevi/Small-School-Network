---
layout: lab
title: Small School Network
description: A full simulation of a  school network built in Cisco Packet Tracer — modelled on a real primary school, covering Layer 2/3 switching, EtherChannel, VLANs, DHCP, wireless LANs and access points.
diagram: https://github.com/user-attachments/assets/2d5e1561-a22b-4e5d-a395-95d55c2b27b3

concepts:
  - Layer 2 & Layer 3 Switching
  - EtherChannel (LACP)
  - VLAN Design
  - SVI Configuration
  - DHCP Server & IP Helper
  - Wireless LAN 
  - Access Points (LAP)
  - Wireless Resilience

objective: >
  The objective of this lab was to simulate a realistic high school network, inspired by the infrastructure
  of a real primary school. The network includes a data centre with core switches, access switches and access points — covering both wired and wireless connectivity across multiple VLANs
  for staff, students, guests, and security.

hardware_image: https://github.com/user-attachments/assets/66739dac-1723-453d-b7ef-76e80a2e7417

hardware:
  - "4x Core Switches (Layer 2 & Layer 3)"
  - "6x Access Switches"
  - "1x Cisco 3560 Switch (added for PoE support)"
  - "4x Access Points (APs)"
  - "1x Wireless LAN Controller (WLC)"
  - "1x DHCP Server"
  - "End devices — PCs, Laptops, Smartphones"

questions:
  - q: Why use EtherChannel between the core switches?
    a: > 
      For physical redundancy. If one wire has an electrical issue, there would still be enough cables to support the network.

  - q: Why use a dedicated DHCP server instead of SVIs for pooling?
    a: >
      [Add your annotation here]

  - q: Why does the server need the ip helper-address command on each SVI?
    a: >
      [Add your annotation here]

  - q: Why does the computer suite need its own dedicated WLAN?
    a: >
      [Add your annotation here]

  

steps:
  - title: Network topology overview
    desc: >
      [Add your annotation here]

  - title: Set up EtherChannel between core switches
    desc: >
      [Add your annotation here]
    code: |
      ! Configure 5 links as a LACP EtherChannel on both sides
      SW(config)# interface range GigabitEthernet0/1 - 5
      SW(config-if-range)# no switchport
      SW(config-if-range)# channel-group 1 mode active
      SW(config-if-range)# no shutdown

      ! Assign IP to port-channel, then bring up
      SW(config)# interface port-channel 1
      SW(config-if)# ip address 10.10.0.254 255.255.0.0
      SW(config-if)# no shutdown

  - title: Convert EtherChannel to Layer 2
    desc: >
      [Add your annotation here]
    code: |
      ! Remove Layer 3 port-channel and recreate as Layer 2
      SW(config)# no interface port-channel 1
      SW(config)# interface range GigabitEthernet0/1 - 5
      SW(config-if-range)# switchport
      SW(config-if-range)# channel-group 1 mode active

  - title: Create VLANs and SVI interfaces
    desc: >
      [Add your annotation here]
    code: |
      SW(config)# vlan 10
      SW(config-vlan)# name Management
      SW(config)# vlan 20
      SW(config-vlan)# name Server
      SW(config)# vlan 30
      SW(config-vlan)# name Student
      SW(config)# vlan 40
      SW(config-vlan)# name Staff
      SW(config)# vlan 50
      SW(config-vlan)# name GuestWiFi
      SW(config)# vlan 80
      SW(config-vlan)# name Security

      SW(config)# interface vlan 10
      SW(config-if)# ip address 10.10.0.1 255.255.0.0
      SW(config-if)# no shutdown
      ! Repeat for VLANs 20, 30, 40, 50, 80 with corresponding IPs

      SW(config)# ip routing

  - title: Configure DHCP server and IP helper
    desc: >
      [Add your annotation here]
    code: |
      ! On each SVI, point DHCP requests to the server
      SW(config)# interface vlan 10
      SW(config-if)# ip helper-address [SERVER_IP]
      ! Repeat for all VLANs

  - title: WLC initial setup via PC browser
    desc: >
      [Add your annotation here]
    code: |
      ! Assign WLC management IP in VLAN 10 subnet
      ! Assign PC an IP in the same subnet
      ! Access WLC via PC web browser, create login, set default gateway to SVI VLAN 10

  - title: Create WLC interfaces per VLAN
    desc: >
      [Add your annotation here]

  - title: Create WLANs and assign to interfaces
    desc: >
      [Add your annotation here]

  - title: Set WLC-connected switch port to native VLAN
    desc: >
      [Add your annotation here]
    code: |
      SW(config)# interface [PORT_CONNECTED_TO_WLC]
      SW(config-if)# switchport mode trunk
      SW(config-if)# switchport trunk native vlan 10

  - title: Add PoE support for LAP
    desc: >
      [Add your annotation here]

  - title: Reception topology and wireless resilience
    desc: >
      [Add your annotation here]

  - title: Switch to standalone APs
    desc: >
      [Add your annotation here]

learnings:
  - "[Add your key learning here]"
  - "[Add your key learning here]"
  - "[Add your key learning here]"
  - "[Add your key learning here]"
  - "[Add your key learning here]"
---
