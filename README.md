
# Welcome to LANCE

Welcome to LANCE.

This document is designed for our partners in the LANCE project.

This document will mostly focus on LANCE Core, as LANCE Freight plugins need to be custom made for each purpose.

### Secure Prim

To use LANCE Core, you must create a secure prim. 

A secure prim is a prim made on an account specifically for your LANCE Vehicles.

This prim will need be the root of every vehicle and must always be NO MOD and NO TRANSFER.

The secure prim serves as your authentication as a real LANCE developer, it pulls the list from a public github.

It is not dependant on servers other than the redundant ones that github has, downtime is almost impossible.

**If you distribute your secure prim MOD and/or TRANSFER, that account will be removed from the whitelist and you will have change the root prim on each vehicle.**

### Activity Pings

Activity Pings are a mechanism that notifies the LANCE Freight server that a player is active.

They are automatically sent when all of the following are true:


- LANCE Core is active.
- Player is seated on the vehicle.
- Vehicle is currently located within the Jeogeot Gulf.

It will ping the server every 3 minutes. 

If the vehicle/player has not sent a ping in the last 5 minutes the system will remove them from the active list.

## Inputs 

### lance on 
Turns the LANCE Core on.

Make sure to sleep for 1 second after sending all the configuration commands so everything gets processed in order.

```
llMessageLinked(LINK_SET, 0, "lance on", ""); 
```

### team
Set the team currently in use. The number field contains the team.
1 is USN, 2 is IJN. 0 unsets the team.
 
```
llLinkedMessage(LINK_SET, 1, "team", NULL_KEY);
```

### startHP
Set the HP which the vehicle starts with. The number field contains the HP.
The default is 500.

_As with any configuration option, this must be set before LANCE Core is activated_

**Any value above 1000 will be silently ignored.**
 
```
llLinkedMessage(LINK_SET, 500, "startHP", NULL_KEY)
```

### bullletDamage
Set the look up list for damage for bullet types. The id must contain a CSV list in the format of damageType,damage.

The default is: 

["BBG",1,"SCG",10,"SMG",10,"SSG",15,"CMG",25,"LMG",10,"HMG",20,"CAN",75,"RKT",100,"FLK",10]

_As with any configuration option, this must be set before LANCE Core is activated_
 
```
llLinkedMessage(LINK_SET, 500, "bulletDamage", llList2CSV[["BBG",1,"SCG",10,"SMG",10,"SSG",15,"CMG",25,"LMG",10,"HMG",20,"CAN",75,"RKT",100,"FLK",10]]);
```
