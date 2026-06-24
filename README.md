# Small-School-Network

Wanted to build a small school network. Have memories of secondary and wanted to visualise the network layout I think it would have.

<img width="437" height="381" alt="image" src="https://github.com/user-attachments/assets/566a4ab4-6c5a-4354-91fc-a808e3c9c413" />


I first started with the 2 core layer 3 switches. I wanted to set up an etherchannel between them as they will serve as the core of the network. To implement redundancy.

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


<img width="542" height="142" alt="image" src="https://github.com/user-attachments/assets/6c471ff0-bc3e-4277-8666-10048247ea9b" />

Ran into issue, i could not make the etherchannel ip so similair to the ip for the svi, so i went back and made the etherchanell layer 2.

I tied to do it to portchaneeel, but ti dodn nto work,

so removed the port channel and recreated it, but as a layer 2 etherchannel, not layer 3

Then enbaled each VLan as svi interface with correspomdng ips and enabled ip routing on both

