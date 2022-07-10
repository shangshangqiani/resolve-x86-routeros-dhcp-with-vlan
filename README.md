# resolve-x86-routeros-dhcp-with-vlan
When a DHCP server and VLAN are created in RouterOS on a computer with the x86 architecture, the log "dhcp server offering lease without success" is displayed.  
This may be caused by the hardware.  
The problem has been existing from 2006 to the present day, and it needs to be resolved.  
# The solution to the problem is described as follows:
An 802.1ad VLAN can be created by interconnecting two RouterOS. That is```Use Service Tag```  
In this way, the DHCP is successfully bound.  
The interconnection between RouterOS and other operating systems must comply with the outer 802.1ad and inner 802.1q, such as OpenWRT.
# The following describes how to interconnect```RouterOS```and```OpenWRT```.
For OpenWRT, we can create ethx.x (802.1q) and ethx.x.x (dual 802.1q) with interfaces.  
However, this is not what we want to achieve.  
After visting the official website at Wiki, we learn that we can run the ip link command.  
```ip link```can be used to view all current interfaces.  
```ip link hlep```can be used to view the help documentation.  
```ip link delete ethx.x.x```can be used to delete an interface.  
The VLAN```format```in OpenWRT is```ethx.Outer layer.inner layer```.  
First, we can run the following command to create the outer vlan proto 802.1ad:  
```yaml
ip link add link eth1 eth1.10 type vlan proto 802.1ad id 10
```
Then, we can run the following command to create the inner vlan proto 802.1q:  
```yaml
ip link add link eth1.10 eth1.10.20 type vlan proto 802.1q id 20
```
At this point, RouterOS can be interconnected.
# Note: In OpenWRT, we can create outer proto 802.1ad only to interconnect with routeros 802.1q VLAN.
