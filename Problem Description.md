I have a client that would like to extend the wireless network covering their facility. Currently they have a network that covers their office (bdlg. A), which has 4 computer terminals, and a corner of an adjacent building (bldg. B), which has a single computer terminal.

My objective is to move building B's existing terminal deeper into the building, closer to where it is needed. To that end I've gotten a router with WDS capability and located it next to the existing terminal in building B. I have experienced problems configuring the routers to work together.


The Hardware setup

I am working with 4 pieces of network hardware:

A) A Motorola NVG510 wireless router/modem (unit A) located in building A. Unit A broadcasts its own SSID (network A), and allows users to connect to a DSL connection provided by AT&T. It currently serves only the nearest part of building B.

- Its wireless IP is 192.168.1.254, subnet 255.255.255.0
- It assigns IP by DHCP in a range from 192.168.1.64 to 192.168.1.128

B) A TPlink TL-WR1043 (unit B) located in building B. It is currently connected to network A via WDS. It provides its own SSDI (network B).

- It has IP 192.168.1.63. subnet address 255.255.255.0
- It is slaved to network A using its built in WDS functionality. Its own DHCP functionality is turned off.
  - When I use unit B on my own home network, the DHCP service of my home router is broadcast correctly through unit B.

C) A computer (Unit C) with a wifi adaptor. It can currentlythat can connect to either SSID A or B.

- It can be configured to use configuration provided by DHCP (config. 1)
- It can be configured to use ip 192.168.1.62 (config. 2)

D) My own wireless enabled smart phone, a samsung galaxy S.  

- This has let me test the signal strength of networks A and B
The problem

In configuration 1, unit C can only connect with unit A.

- Unit B does not pass through Unit A's DHCP service, as it does when configured at my home.

In configuration 2, unit C can connect to both unit A and B.

- If connected to unit A, unit C can ping both unit A and unit B.
  - It can log into web control panels for both unit A and B.

- If connected to unit B, unit C can ping unit B and log into its control panel
  - It cannot, however, ping unit A or log into its control panel. 


What I've learned so far

The NVG510 has known problems with WDS. Ron Berman writes about these problems in his [blog post](http://www.ron-berman.com/2011/11/24/motorola-nvg510-help-page-for-att-u-verse-users/).

 


The Goal

I'd like to extend the range of unit A by using unit B in a WDS capacity. Unit C should be able to connect 


What I've tried

What happened
