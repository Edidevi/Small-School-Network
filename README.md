# Small-School-Network

Wanted to build a small school network. Have memories of secondary and wanted to visualise the network layout I think it would have.

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/131e5889-8501-4f01-b19a-4c1c162b4cc4" />

I first started with the 2 core layer 3 switches. I wanted to set up an etherchannel between them as they will serve as the core of the network. To implement redundancy.

<img width="750" height="273" alt="image" src="https://github.com/user-attachments/assets/2d5e1561-a22b-4e5d-a395-95d55c2b27b3" />

I set up 5 wired connections between the two.
I used the `interface range` command to configure the connections together. I then used `no switchport` to disable  layer 2 routing and used `channel-group 1 mode active` to turn connection to LACP etherchannel called channel 1.
<img width="350" height="100" alt="image" src="https://github.com/user-attachments/assets/7b1fe841-c765-498d-81cc-edbad1eb2652" />

I did this for both sides of the etherchannel connection. Port-channel on both sides was still down, so i assigned the correct ip address `10.10.0.254` to the interfaces, then typed `no shutdown` on the physical interface range.
The link was now up between the two.

The next step was to create the virtual interfaces. I established the VLANs on the switches.
<img width="371" height="150" alt="image" src="https://github.com/user-attachments/assets/dfafa7f8-5086-42c1-8e8c-f4f843a03db8" />
