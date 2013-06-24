# Extending an existing ATT Uverse connection using WDS

I have a client (a machine shop facility) that would like to extend the wireless network covering their facility. Currently they run a wireless router provided via the ATT Uverse service (more about the exact hardware later.) It covers their office (bdlg. A), which has 4 computer terminals, and a corner of an adjacent building (bldg. B), which has a single computer terminal.

My given objective is to move building B's existing terminal deeper into the building, closer to the work area where it is needed. To that end I've gotten a router with WDS capability and located it next to the existing terminal in building B. I have experienced problems configuring the router to work together.


## The Hardware setup

I am working with 4 pieces of network hardware:

A) A Motorola NVG510 wireless router/modem (*unit A*) located in *building A*. Unit A broadcasts its own SSID (*network A*), and allows users to connect to a DSL connection provided by AT&T. It currently serves only the nearest part of *building B*.
- Its wireless IP is 192.168.1.254, subnet 255.255.255.0
- It assigns IP by DHCP in a range from 192.168.1.64 to 192.168.1.128
- It uses WPA2/PSK

B) A TPlink TL-WR1043 (*unit B*) located in building B. It is currently connected to network A via WDS. It provides its own SSDI (*network B*).
- It has IP 192.168.1.63. subnet address 255.255.255.0
- It is slaved to network A using its built in WDS functionality. Its own DHCP functionality is turned off.
  - When I use unit B on my own home network, the DHCP service of my home router is broadcast correctly through unit B.
- It shares the same WPS2/PSK key as network A.

C) A computer (*Unit C*) with a wifi adaptor. It can current connect to either SSID A or B.
- It can be configured to use configuration provided by DHCP (config. 1)
- It can be configured to use ip 192.168.1.62 (config. 2)

## The problem

In configuration 1, unit C can only connect with unit A. Unit B does not pass through Unit A's DHCP service, as it does when configured in my home test network.

In configuration 2, unit C can connect to both unit A and B. When connected to unit A, unit C can ping both A and  B, and it can access their web based control panels. If connected to unit B, unit C can ping unit B and log into its control panel. It cannot, however, ping unit A or log into its control panel. 


## What I've learned so far

The NVG510 has known problems with WDS. Ron Berman writes about these problems in his [blog post](http://www.ron-berman.com/2011/11/24/motorola-nvg510-help-page-for-att-u-verse-users/). Following links and related searches I find some [tricky solutions](http://forums.att.com/t5/Features-and-How-To/NVG510-Bridge-Mode/m-p/2928989#M29846) which include disabling unit A's IPv6 capabilities, and configuring unit B to use unit A's external IP as its gateway. I do not understand the solution, and it is not evident to me that these would solve the existing problem with WDS, since they seem to assume unit A and B can be connected via a cable.


### Likeyly Fix
It occurs to me that I could purchase an additional router (unit D) and locate it in building A. I would then use WDS via unit B to extend unit D's coverage to the required areas.

## My questions

### What could be causing the strange ping behavior?
Unit B is connected to network A - any other unit connected to network A can see unit B's IP. B can also ping B's IP. But for whatever reason, pings cannot tunnel from network B to reach targets on network A. Namely, units on network B cannot use router A as their gateway. This behavior runs against what I saw when I connected unit B to a home network, where pings were able to pass freely across router B.

### Is the 'likely fix' likely to work?
I could spend some more money, and purchse an additional router unit for building A. This might not be a bad solution. If the problem is that unit A is simply incompatible with WDS, this might turn out to be the best solution. I would use a cheaper TP-link router for this, to ensure compatibility with unit B.

### Should I try some of the trickier solutions I've found with regards to the NVG510?
I worry this might take more time to iron out. I'm also concerned about destabilizing unit A's existing functionality and causing downtime in the Bldg A office.
