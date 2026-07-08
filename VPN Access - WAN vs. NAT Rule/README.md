# Accessing WireGuard VPN: Using WAN vs. NAT Port Forward Rule

Something I did not think about the first time I set up my WireGuard VPN was how I, or anyone, was going to connect to it. It just was not something that I had thought about; all my previous experiences with VPNs involved me just hitting a button and it just working. Thankfully, the guide I was following taught me that you would need to forward the port you assigned the instance to listen to. Easy enough, but then I realized that there's technically two ways to go about doing that: one involves adding a rule to your WAN firewall rules, and the other is the traditional port forward via a NAT rule. 

## WAN RULE - STEPS:
1. Firewall > Rules > WAN
2. Add a rule with the following:
   1. Action: **Pass**
   2. Interface: **WAN**
   3. Direction: **In**
   4. Protocol: **UDP**
   5. Destination: **WAN Address / Public IP Address**
   6. Destination Port Range: **from: *listen-port* / to: *listen-port***
3. Save all changes

> [!NOTE]
> WireGuard requires more than just this one (or more) rule! Make sure to add the appropriate **outbound NAT rule**, for example.

## NAT PORT FORWARD RULE - STEPS:
1. Firwall > NAT > Port Foward
2. Add a rule with the following:
   1. Protocol: **UDP**
   2. Destination: **WAN Address / Public IP Address**
   3. Destionation Port Range: **from: *listen-port* / to: *listen-port***
   4. Redirect Target IP: **WireGuard Instance IP**
   5. Redirect Target Port: **WireGuard Instance Listen Port**
3. Save all changes

## What's the Difference?

From what I understand, the WAN rule(s) are to be used whenever you have a service running directly on the router. In my case, since I have WireGuard hosted directly in OPNsense, this is what I use. However, if you have a service running on a server inside of your internal network, then you would use the NAT Port Forward rule(s). For example, if I wanted to access my movies and shows that I have using JellyFin, I can use this method to be able to access the service from wherever. However, doing this isn't exactly safe since your opening up more ports that is safe to do so, which is why I set up my VPNs in the first place. 

## REFERENCES
[1] https://homenetworkguy.com/how-to/configure-wan-and-nat-port-forward-rules-in-opnsense/