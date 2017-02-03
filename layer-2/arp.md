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

## Note
If the device that you're looking for isn't in the arp translation list, then the two devices haven't had to talk to each other. If you want to force all devices on the same subnet to "phone home", you can ping the broadcast address `x.x.x.255`.


## More tools to research
- `arping`
