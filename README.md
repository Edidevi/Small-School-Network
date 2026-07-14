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
| Staff VLAN       | 10.40.0.0 - 10.40.0.255    | 40       | 10.40.0.1             | /24  |
| Guest WiFi VLAN  | 10.50.0.0 - 10.50.0.255    | 50       | 10.50.0.1             | /24  |
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

Set up Wirleess contorllers, i wanted to have a centralised wireless lan contorller (WLC) with many different aps around the area, to extend the service range








wanted to use same wlc for computer suite, but due to amount of computers want computer to have uts own deducatwd wlanThat's a good design decision, and it's one that many schools actually use.

A computer suite has many more devices, higher bandwidth demand, and different security requirements than reception, so giving it its own WLAN makes sense.

Before this, needed to sort out etherchannle between switch and wlan, for redundancy reasons.
To do thi, have to edit wlc by PC

First assigned wlc management and PC ips in the management subnet

Brought pc into enviormnet , assigned ip in same subent as wlc, then used pc web browser to reach wlc
<img width="1881" height="713" alt="image" src="https://github.com/user-attachments/assets/e929aa7c-e7a9-4974-9626-f750908904da" />


<img width="1020" height="518" alt="image" src="https://github.com/user-attachments/assets/25c48aee-8136-4cb1-9058-9a08007cd431" />
Logged in, creating id and password, then asinged correct detiales, aswell as using the SVI vlan 10 as the default gateway.

i then created the guest and employee networks. 
'
<img width="622" height="453" alt="image" src="https://github.com/user-attachments/assets/45d6edaa-7afc-4b46-aa1e-ef45c14c50ec" />





Chathamwireless
Chatham1*

Staff wifi

Securesenior1*


<img width="705" height="507" alt="image" src="https://github.com/user-attachments/assets/d5db22e3-bda4-44bd-8316-516f080579da" />

After set up, i could no longer ping the wlc, i realised that i had palced staff wifi and management ip on vlan 10, so i needed to make the ports access ports and tag them accordingly

<img width="911" height="617" alt="image" src="https://github.com/user-attachments/assets/d78a24df-93df-45fb-8a05-d5b3e03e2f2d" />

After setting up, wanted to put aps, next to smartphone. I was inspired by mi girfirend, she works in health and she said in the hospital they had cisco phones. It allows the recpetionist to answer calls and not always be desk bound, through roaming via the many aps in the network.

<img width="492" height="212" alt="image" src="https://github.com/user-attachments/assets/ff51e178-719b-4278-9718-ee9678f2cfab" />

I updated the recpetion topology, to reflect a common recpetion layout, with a sliding window in front of two recpetionists with computers, and a smartphone at the back for them to pick up when one of them wants to leave the desk for some reason, such as event coordination, guiding guests or office supply restocking.

The next thing was the ip addresses, instead of using the  svis for dhcp pooling, the server was the chosen choice for more resiliency.
<img width="1725" height="897" alt="image" src="https://github.com/user-attachments/assets/9205cdbf-42dc-4278-aa5d-a16d69441f36" />

<img width="847" height="622" alt="image" src="https://github.com/user-attachments/assets/afa6dd7e-2b95-48f9-a328-1f9d49f8b425" />

<img width="507" height="217" alt="image" src="https://github.com/user-attachments/assets/77331d4a-05aa-45ab-afba-001a464b3f50" />

<img width="738" height="168" alt="image" src="https://github.com/user-attachments/assets/0df9edc8-131a-44ce-b2e2-128e702d9d08" />

learnt that server cna;t be attacjed to trunk. Wont be an issue managing multplie vlans becase of th eip helper command that i configured

<img width="431" height="107" alt="image" src="https://github.com/user-attachments/assets/363e139f-53d9-41a4-9caa-cb7d32532267" />

<img width="822" height="255" alt="image" src="https://github.com/user-attachments/assets/72fbed5e-898f-422b-8507-ca2e0c532dc6" />

LAP wan't turing on, so 

<img width="268" height="132" alt="image" src="https://github.com/user-attachments/assets/e8b91363-1f73-42e9-b035-f2e80f0c548c" />

<img width="606" height="471" alt="image" src="https://github.com/user-attachments/assets/7566ee35-08a0-4355-8119-9e7ffb0cd973" />

had to add a 3560 switch as other switch did not have

<img width="662" height="551" alt="image" src="https://github.com/user-attachments/assets/d5ee901d-86ad-4672-9b1c-b30be728e53d" />

Added power adaptor to ap.

<img width="786" height="190" alt="image" src="https://github.com/user-attachments/assets/cdf68287-27cc-412d-9ec1-d91c1acf9e6e" />

<img width="867" height="627" alt="image" src="https://github.com/user-attachments/assets/04020775-58fc-45d1-b420-fdbbba08f864" />


Then need to set up pool for staff wifi

<img width="841" height="626" alt="image" src="https://github.com/user-attachments/assets/a5d3d0ee-5448-4bc9-997f-d00d4b8ea9bd" />
added the wlc address to pool

Then went bakc to make svi interface 40 to enable ip helper for the server ip

<img width="827" height="245" alt="image" src="https://github.com/user-attachments/assets/d11bda2f-7f05-44d2-bf12-6a9515dcec38" />

I struggled with this concept, i wanted to change the access port for the ap to the stqaff wifi vlan

<img width="1202" height="556" alt="image" src="https://github.com/user-attachments/assets/6f310949-1109-4a70-9910-88bbaf43860d" />

I then changed interface connected to wlc to native vlan, then ping worked as wlc does not aumotatically tag frames so switch qas dropping them

then logged into wlc and started creating interfaces for the sepereate wlans...


<img width="677" height="472" alt="image" src="https://github.com/user-attachments/assets/23ad7f0f-c803-4993-bb69-3037dcfa2c86" />

For each vlan i sepcified the following,

For the dhcp server, i realised that had to be set for every interface. I took this as an opportunity to create all the necessary pools and ip helper default gateways for every vlan.

<img width="1738" height="432" alt="image" src="https://github.com/user-attachments/assets/d4cbac46-3926-43d5-8d53-4ada438a75e1" />

THen went back to create all required interfaces

I also noted httat in creating the interfaces, i gave them the first ip of the subnet, so i went back to the dhcp pool to ammend the pooling to start after that.

then i came back and created all corresponding interfaces 

<img width="1212" height="216" alt="image" src="https://github.com/user-attachments/assets/039ff55d-b5b3-485e-8f45-1696d460a828" />

Next setp is to create wlans that correspond with the wireless interfaces 
