# Extending an existing ATT Uverse connection using WDS

I have a client (a machine shop facility) that would like to extend the wireless network covering their facility. Currently they run a wireless router provided via the ATT Uverse service (more about the exact hardware later.) It covers their office, which has 4 computer terminals, and a corner of an adjacent building, which has a single computer terminal.

My given objective is to move building B's existing terminal deeper into the building, closer to the work area where it is needed. To that end I've gotten a router with WDS capability and located it next to the existing terminal in building B. I have experienced problems configuring the routers to work together.


## The Hardware setup

I am working with 3 pieces of network hardware:

**A.** A Motorola NVG510 wireless router/modem ( **unit A** ) located in **building A**. Unit A broadcasts its own SSID ( **network A** ), and allows users to connect to a DSL connection provided by AT&T. It currently serves only the nearest part of building B.
- Its wireless IP is 192.168.1.254, subnet 255.255.255.0.
- It assigns IP by DHCP in a range from 192.168.1.64 to 192.168.1.128.
- It uses WPA2/PSK encryption.

**B.** A TP-Link TL-WR1043 ( **unit B** ) located in **building B**. It is currently connected to network A via WDS. It provides its own SSID ( **network B** ).
- It has IP 192.168.1.63. subnet address 255.255.255.0.
- It is slaved to network A using its built in WDS functionality. Its own DHCP functionality is turned off.
  - When I test unit B on my own home network, this configuration allows my home DHCP service to broadcast correctly to clients connected via unit B.
- It shares the same WPS2/PSK key as network A.

**C.** A computer ( **Unit C** ) with a wifi adapter. It can current connect to either SSID A or B.
- It can be configured to use configuration provided by DHCP ( **configuration 1** .)
- It can be configured to use IP 192.168.1.62 ( **configuration 2** .)

## The problem


Configuration 2
: Unit C can connect to both unit A and B. 

: When connected to unit A, unit C can ping both A and  B, and it can access their web based control panels. It correctly uses A as its Internet gateway. 
: When connected to unit B, unit C can ping unit B and log into its control panel, but it cannot ping unit A or log into its control panel. 

Configuration 1
: Unit C can only connect with unit A.
: Unit C cannot acquire a network address via unit B. Unit B's DHCP is turned off, and unit A's DHCP seems to be inaccessible to units on network B.

## What I've learned so far

The NVG510 has known problems with WDS. Ron Berman writes about these problems in his [blog post](http://www.ron-berman.com/2011/11/24/motorola-nvg510-help-page-for-att-u-verse-users/). Following links and related searches I find some [tricky solutions](http://forums.att.com/t5/Features-and-How-To/NVG510-Bridge-Mode/m-p/2928989#M29846) which include disabling unit A's IPv6 capabilities, and configuring unit B to use unit A's external IP as its gateway. I do not understand the solution, and it is not evident to me that these would solve the existing problem with WDS, since they seem to assume unit A and B can be connected via a cable.


### Likely Fix
It occurs to me that I could purchase an additional router ( **unit D** ) and locate it in building A. I would then use WDS via unit B to extend unit D's coverage to the required areas.

## My questions

### 1. What could be causing the strange ping behavior?
It is evident that unit B is connected to network A. Any other unit connected to network A can ping unit B's IP. Similarly, any unit connected to network B can also ping B's IP. But for whatever reason, pings cannot tunnel from network B to reach targets on network A. I'd assume that pings from network A cannot reach IPs on network B, though I have yet to test this. 

The end behavior seems to be that units on network B cannot use router A as their gateway. This behavior runs against what I saw when I connected unit B to a home network, where pings and other traffic were able to pass freely across router B.

### 2. Is the 'likely fix' likely to work?
I could spend some more resources, and purchase an additional router unit for building A. If the problem is that unit A is simply incompatible with WDS, this might turn out to be the best solution. I would use a cheap TP-Link router for this, to ensure compatibility with unit B.

### 3. Should I try some of the trickier solutions I've found with regards to the NVG510?
I worry this might take more time to iron out. I'm also concerned about destabilizing unit A's existing functionality and causing downtime in the building A office.

# Resolutions

## A Quick Chat

After a talk with Wiskey`Wonka of the ##Networking@Freenode IRC channel, a few things became apparent.

### Inter-brand WDS is [hit or miss](https://en.wikipedia.org/wiki/Wireless_distribution_system#Implementations).
- The WDS standard is IEEE 802.11-1999 and specifies a 4-adress frame protocol that makes WDS possible.
- It does not define how WDS implementations are to be constructed or how WDS stations should interact.

### Using the OpenWRT wireless firmware may solve the problem.
- This is reasonable, as this would provide a consistent WDS implementation.
- This is not a good solution in this case, as I cannot risk damage to the existing Uverse router.
- It may be a good solution in future projects.

### The best bet is to use another unit of the same manufacturer and chipset
- I'd preferably use the exact same model, and adjoin it to unit A via an ethernet connection.
- This seems like the lowest risk solution.

### Conclusions
1. I'm inclined to shy away from solutions requiring modifications to the existing router. If the router gets damaged in my modifications, it will cause outages (and resulting losses) that I cannot cover. 
2. The strange behavior may be caused by mismatches in the router chipsets. I was lucky that the router worked at home.
3. To my though, the soundest solution is to purchase an additional TP-link router and deploy it building A.
  - This will cut into my profit, but is the most appealing solution from a support perspective.
