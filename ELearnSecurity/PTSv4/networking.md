Networking - PTSv4

# Networking
### Terminologies and Theory



**Packets**

In networking, data is carried by packets. They are streams of bits.

They transmit through physical media like wires in LAN or the air in WiFi.



![Structure of a packet](/ELearnSecurity/PTSv4/assets/packet-structure.png)



HEADER: The header is protocol-		specific. It ensures that 			receiving host can correctly 		interpret and handle the 			communication.

PAYLOAD: It is the actual data which gets transmitted during communication in a network.

**Encapsulation**

Every protocol or OSI model layers has its own payload so in order for them to co-operate, encapsulation is done.

Each time data is transported to another layer, the previous HEADER and PAYLOAD adds up and makes up just the PAYLOAD so the layer gets the chance to have its own HEADER at place.



**Reserved IPv4 Addresses**

0.0.0.0 - 0.255.255.255 representing public network.

127.0.0.0 - 127.255.255.255 representing localhost i.e. this computer.

192.168.0.0 - 192.168.255.255 representing private network.


- > Method to take network prefix from subnet:
<br> - Convert IP and subnet mask to binary form and perform bitwise __AND__ on them to get the binary format of network prefix.


***CIDR (Classless Inter-Domain Routing)**

It is the IP range which can be used to specify a list of IP address under that subnet mask. Example - 192.168.3.0/24

- > Method to calculate number of maximum hosts in a CIDR range:
<br> - Convert subnet mask to binary and perform bitwise __NOT__ operation and use the value to perform __AND__ operation with the binary value of IP address. Note the number of bits of the result and use it as a power to 2. 2^number-of-bits will give you number of hosts that the network can contain.


