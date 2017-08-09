
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

### Misc

**Vehicle description must be set to LANCE" for ammunition to work**

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

### bulletDamage
Set the look up list for damage for bullet types. The id must contain a CSV list in the format of damageType,damage.

The default is: 

["BBG",1,"SCG",10,"SMG",10,"SSG",15,"CMG",25,"LMG",10,"HMG",20,"CAN",75,"RKT",100,"FLK",10]

_As with any configuration option, this must be set before LANCE Core is activated_
 
```
llLinkedMessage(LINK_SET, 500, "bulletDamage", llList2CSV[["BBG",1,"SCG",10,"SMG",10,"SSG",15,"CMG",25,"LMG",10,"HMG",20,"CAN",75,"RKT",100,"FLK",10]));
```

### bombDamage
Set the look up list for damage for bomb types. The id must contain a CSV list in the format of damageType,damage.

The default is: 

["CMB",25,"SMB",75,"MDB",200,"LGB",500,"DCG",200,"LBB",75,"MBB",125,"HBB",200,"TRP",375,"KMK",300]

_As with any configuration option, this must be set before LANCE Core is activated_
 
**TRP and DCG are special values. TRP will only do damage if the hitKey is passed along and does damage in full. DCG has special scaling on the Z-Axis**

```
llLinkedMessage(LINK_SET, 500, "bombDamage", llList2CSV[["CMB",25,"SMB",75,"MDB",200,"LGB",500,"DCG",200,"LBB",75,"MBB",125,"HBB",200,"TRP",375,"KMK",300]));
```

### outgoingReward
Enables and sets the requirements for the kill rewards. 
This allows vehicles to communicate back to vehicles like the submarine that they have been taken down and awards the right amount of points.

The number field holds the to be awarded points and the id fields holds a CSV list on which killed by ammotype it should send the message (IE: TRP, LBB)

_As with any configuration option, this must be set before LANCE Core is activated_

**Any reward above 20000 will be silently ignored.**
 
```
llLinkedMessage(LINK_SET, 1000, "outgoingReward", "TRP, LBB");
```

### enableIncomingReward
Enables the incoming rewards for killing other vehicles. This should only be used on submarines and aircraft intended to destroy Freighers.
The default is 0(FALSE/OFF).

_As with any configuration option, this must be set before LANCE Core is activated_

**The only options are 0 and 1.**
 
```
llLinkedMessage(LINK_SET, 1, "enableIncomingReward", NULL_KEY)
```

### rewardPassthrough
This is a special input, since the projectiles use OBJECT_REZZER_KEY to get the origin since this is more reliable than doing both OBJECT_REZZER_KEY and OBJECT_ROOT in succesion or using llShout() on object_rez, you will need to have your object pass through values recieved by email for the backup email system. 

**Use supplied email snippet to fetch new emails.**
 
```
llLinkedMessage(LINK_SET, 0, "rewardPassthrough", "EMAILCONTENTSGOHERE")
```

## Output

### hp
The sensor will send out a linked message with the string being "hp" and the number being the current health whenever it's updated.

### crash
The sensor will send out a linked message with the string being "crash" once the vehicle HP reaches 0. 

### authcode
The sensor will send out a linked message with the string being "authCode" and the number being the authcode you need to use a start_param while rezzing a projectile. 

## Bomb Projectile example script
Next to the supplied bomb script, you need to have your own to guide it.

Here's an example:

```
FX() {
	// FX here
}

default {
	collision_start(integer num_detected) {
		while(num_detected--) {
			string description = llList2String(llGetObjectDetails(llDetectedKey(num_detected), [OBJECT_DESC]),0);
			if (llDetectedType(num_detected) & AGENT || description == "LANCE") {
				llMessageLinked(LINK_THIS,0,"explode",llDetectedKey(num_detected));
			}
		}
		llMessageLinked(LINK_THIS,0,"explode",NULL_KEY);
		FX();
	}
	land_collision_start(vector vec) {
		llMessageLinked(LINK_THIS,0,"explode",NULL_KEY);
		FX();
	}
}
```
**The LANCE bomb script will handle turning phantom/physical on explode.**
