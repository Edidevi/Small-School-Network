# Nursery-Network

Wanted to build a small school network. Have memories of secondary and wanted to visualise the network layout I think it would have.

<img width="437" height="381" alt="image" src="https://github.com/user-attachments/assets/566a4ab4-6c5a-4354-91fc-a808e3c9c413" />

I decided to move to cisco devnet sandbox. THis type of implementation would be too tedious to do by hand. It omnly makes sense to replace manual work with automation when needed.



<img width="750" height="273" alt="image" src="https://github.com/user-attachments/assets/2d5e1561-a22b-4e5d-a395-95d55c2b27b3" />

I set up 5 wired connections between the two.
I used the `interface range` command to configure the connections together. I then used `no switchport` to disable  layer 2 routing and used `channel-group 1 mode active` to turn connection to LACP etherchannel called channel 1.

I did this for both sides of the etherchannel connection. Port-channel on both sides was still down, so i assigned the correct ip address `10.10.0.254` to the interfaces, then typed `no shutdown` on the physical interface range.
The link was now up between the two.

<img width="451" height="157" alt="image" src="https://github.com/user-attachments/assets/1e5abb4d-8d54-4708-baa8-60aad776172a" />


The next step was to create the virtual interfaces. I established the VLANs and the corresponding layer 3 interfaces on the switches.

| VLAN Name        | IP Range                   | VLAN ID  | Default gateway (SVI) | CIDR |
|------------------|----------------------------|----------|-----------------------|------|
| Management VLAN  | 10.10.0.0 - 10.10.255.255  | 10       | 10.10.0.1             | /16  |
| Server VLAN      | 10.20.0.0 - 10.20.0.255    | 20       | 10.20.0.1             | /24  |
| Student VLAN     | 10.30.0.0 - 10.30.255.255  | 30       | 10.30.0.1             | /16  |
| Staff WiFi VLAN  | 10.40.0.0 - 10.40.0.255    | 40       | 10.40.0.1             | /24  |
| Guest WiFi VLAN  | 10.50.0.0 - 10.50.0.255    | 50       | 10.50.0.1             | /16  |
| Printers VLAN    | 10.60.0.0 - 10.60.0.255    | 60       | 10.60.0.1             | /24  |
| Voice VLAN       | 10.70.0.0 - 10.70.255.255  | 70       | 10.70.0.1             | /16  |
| Security VLAN    | 10.80.0.0 - 10.80.0.255    | 80       | 10.80.0.1             | /24  |

Ran into issue, i could not make the etherchannel ip so similair to the ip for the svi, so i went back and made the etherchanell layer 2.

I tied to do it to portchaneeel, but ti dodn nto work,

so removed the port channel and recreated it, but as a layer 2 etherchannel, not layer 3

Then enbaled each VLan as svi interface with correspomdng ips and enabled ip routing on both

Ip configuration would have neen too tedious to do by hand, so introduced server for automation.

<img width="1015" height="785" alt="image" src="https://github.com/user-attachments/assets/0920744f-01f9-4b36-80eb-b8722f6b226e" />


<img width="748" height="512" alt="image" src="https://github.com/user-attachments/assets/b4416e33-c01b-415d-ad68-389c560b2851" />

<img width="620" height="232" alt="image" src="https://github.com/user-attachments/assets/654e5f30-45d5-4359-9353-5afdba3e387f" />

<img width="960" height="401" alt="image" src="https://github.com/user-attachments/assets/07ce9905-e787-4634-b523-632a6ee183d2" />


<img width="823" height="662" alt="image" src="https://github.com/user-attachments/assets/77ef8f5d-a0a3-41c0-9933-7c9dd2397e35" />

<img width="775" height="385" alt="image" src="https://github.com/user-attachments/assets/682f4a6b-caab-404d-af40-daded9f563b1" />

<img width="1550" height="696" alt="image" src="https://github.com/user-attachments/assets/69a76faf-1516-4baa-a7f4-b5715e4669e1" />

I then set up an ap and a backup ap for the recpetion. Backup ap would be connected to the backup switch. I turned of ap, so if one ap is not working,  the switch can be connected to the other ap.

<img width="1260" height="602" alt="image" src="https://github.com/user-attachments/assets/c61645e6-b96c-4b25-8fd5-4f84fcda58a6" />

Chose the specific WLC as it had multiple orts, allowing me ti impleemnt wireles resiliency.

Connected main switch, main ap, backup switch and backup ap all together.
THen i needed to confiure the etherchannels then move toward setting uo the wireless lan and its corresponding aps.





wanted to use same wlc for computer suite, but due to amount of computers want computer to have uts own deducatwd wlanThat's a good design decision, and it's one that many schools actually use.

A computer suite has many more devices, higher bandwidth demand, and different security requirements than reception, so giving it its own WLAN makes sense.

