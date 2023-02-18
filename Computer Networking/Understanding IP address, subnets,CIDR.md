### Network Address Translation:
To rewrite the address when packets traverse network **borders** and still keep the correct destination. It allows the **same IP address** to be used on **multiple isolated** networks. 

### IPV4 address format:
Contains 4 segments of 8 bits -> in total 32 bits

Divided into 2 parts:
1st part: identify the network
2nd part: identify the host within the network identified in 1st part

#### Address classes and reserved range
> The division of large portions of IP space into classes is now a legacy concept

|  Class 	|   First 4 bits	|   IP range	|   Network vs. Host	| Notes |
|---	|---	|---	|---	|---	|
|  A 	|   0---	|   0.0.0.0 - 127.255.255.255	|  Network: Remainder 3 bits of 1st Octet <br>Host: The rest bits	| So 2^3 permutation for network |
|  B	|   10--	|   128.0.0.0 - 191.255.255.255	|  Network: Remainder 2 bits + full of 2nd Octet<br>Host: The rest bits| So 2^(2+8) permutation for network |
|  C 	|   110-	|   192.0.0.0 - 223.255.255.255	|  Network: Remainder 1 bits + entire 2nd, 3rd Octet<br>Host: The rest bits	| So 2^(2+8+8) permutation for network |
|  D 	|   1110	|   224.0.0.0 -	239.255.255.255|   	|
|  E 	|   1111	|   240.0.0.0 - 255.255.255.255	|   	|

Notes:
0000 0000
0111 1111 = 127



### IPV6 address format:
Contains 8 segments of 4 hexadecimal (1 hexadecimal = 4 bits e.g. f = 1111) -> in total 128 bits

### Subnetting, Netmask, #### Subnet mask

>Subnetting:  The process of dividing a network into smaller network sections

> Netmask: Tells the amount of address bits used for network portion in an IP address

> Subnet mask: Default, a network has only one subnet, that contains all the host addresses defined within; A subnet mask is to further divide the network

For an IP address e.g. 
`192.168.0.15` 
-> Class C Network
-> First 3 Octets are for network portion 
	`192.168.0.15` -> 1100 0000 - 1010 1000 - 0000 0000 - 0000 1111
	`255.255.255.0` ->1111 1111 -     1111 1111    - 1111    1111 - 0000 0000

To determine network portion, by applying bitwise `AND` [^1] operation between the **address** and the **netmask**, so it becomes:
1100 0000 - 1010 1000 - 0000 0000 - 0000 0000 -> `192.168.0.0` ->the network portion

[^1]: After performing the `AND` operation, network portion address will be saved and host portion will be discarded.

==Subnetting is to take a portion of host space of an address, and use it as additional networking specification to divide the address space again==

so rather than `255.255.255.0` be the netmask, we take one more bit as the network portion, it becomes:

1111 1111 - 1111 1111 - 1111 1111 - 1000 0000 -> perform the `AND` again -> `192.168.0.128`

As a consequence,
1st subnet : 192.168.0.1 to 192.168.0.127
2nd subnet: 192.168.0.129 to 192.168.0.255 

192.168.0 is fixed, as it is class C ip address remember?

### CIDR as alternative to traditional subnetting
> Classless Inter-Domain Routing: add a specification in the IP address itself as to the number of significant bits that make up the networking(routing) portion

IP address `192.168.0.15` with `255.255.255.0` in traditional notation = `192.168.0.15/24` in CIDR notation

