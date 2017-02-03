# ARP

Address resolution protocol is a way of converting an ip address to a mac address.

## `arp`

In it's simplist usage, `arp` converts an ip to a mac.
```bash
$ arp 192.168.1.2
? (192.168.1.2) at f4:5c:89:b9:3c:7f on en0 ifscope permanent [ethernet] 
```

`arp` can also show the entirety of its cached mac translation table:
```bash
$ arp -a
? (192.168.1.2) at f4:5c:89:b9:3c:7f on en0 ifscope permanent [ethernet] 
```

Delete an entry from the arp table with `arp -d`:
```bash
$ arp -d 192.168.1.2
```

### Note
If the device that you're looking for isn't in the arp translation list, then the two devices haven't had to talk to each other. If you want to force all devices on the same subnet to "phone home", you can ping the broadcast address `x.x.x.255`.

## `arping`

`arping` is very similar to `ping`, but instead of taking an ip and sending an ICMP ping, it takes an ip or mac (if an ip is passed, it performs arp to convert that ip to a mac) and sends a `who-has` request to that mac address to determine if the host responds.

- `-S`: Pick an arbitrary ip to make the requests from.
- `-c`: Only send a specified number of requests to the system being `arping`ed.
- `-C`: Only wait for a specified number of replys back from the system being `arping`ed.
- `-p`: Turn on promiscous mode. This lets you listen to all traffic on the network. 

For example, if you don't yet have an ip but you know that someone else has the ip `192.168.1.2` and the mac `00:00:00:00:00:00`, you can "borrow" their ip and ping another system:
```bash
$ arping -S 192.168.1.2 -s 00:00:00:00:00:00 -p 01:02:03:04:05:06 # "ping" the mac 01:02:03:04:05:06
```

If you change the ip a mac is mapped to, you can update all nodes on the network with the new ip => mac mapping:
```bash
$ # NOTE: test me!!!!!!
$ arping -A -I eth0 172.16.42.161
```

