# Information

TCP provides reliable, ordered, and error-checked delivery of a stream of octets between applications running on hosts communicating by an IP network.

# Concepts:

`CIDR Block`

`ICMP` / `TCP` / `UDP`

`ports`

`listening on a port` / `receiving on a port`

`network interfaces

# Tools

### `nc` - netcat lets you send or receive arbitrary data over tcp.

Example: 
```bash
$ echo "foo bar baz!" | nc some-server.com 4321
```

netcat also has a listening mode:
```bash
$ on `some-server.com`:
$ nc -l 4321
foo bar baz!
```

### `nmap` - nmap lets you scan for available ips and open ports on those ips.

Example:

Scan for all available ips that match a given CIDR block.
```bash
$ nmap -sn 192.168.1.0/24
```

Scan for all ips with the given ip open:
```bash
$ nmap -p 22 192.168.1.0/24
```

### `ngrep` - search through the content of all tcp requests that touch a machine and search through them.

Format of each entry:
```
T 192.168.1.5:62425 -> 1.2.3.4:80
..........data..........
```

- `T`: This is either `T` or `U`, for TCP or UDP.
- `192.168.1.5`: The computer the request is coming from (in this case, my laptop)
- `62425`: The request's source port
- `1.2.3.4`: The computer the request is being made to
- `80`: The request's destination port

Super simple example:
```bash
$ # after I ran this command, I ran `curl example.com` in another terminal.
$ sudo ngrep -d any example.com
interface: any
match: example.com
########
U 192.168.30.125:57211 -> 8.8.8.8:53   # here's a dns request. Notice the `U` - this is made over UDP.
  .............example.com.....                                                                      
#
U 192.168.30.125:61365 -> 8.8.8.8:53   # here's another dns request
  .............example.com.....                                                                      
#
U 8.8.8.8:53 -> 192.168.30.125:57211
  .............example.com..............u..].. # the first dns request's response
#
U 8.8.8.8:53 -> 192.168.30.125:61365
  .............example.com.............>...&.(.. ...H..%..F # The other dns request's response.
####
T 192.168.30.125:49575 -> 93.184.216.34:80 [AP]   # Finally, the actual request.
  GET / HTTP/1.1..Host: example.com..User-Agent: curl/7.51.0..Accept: */*....                        
#####################################################################################################################^Cexit
132 received, 0 dropped
```

Scan on a particular interface:

Use `-d`:
```bash
$ sudo ngrep -d eth0 example.com
```

Also, it's good to know that ngrep takes it's filter in [Berkley packet format](https://biot.com/capstats/bpf.html). Not necisarily required to use the tool, but a good thing to know.

### `ping`: Send a ICMP ping to a given host, and look for a response.

Example:
```bash
$ ping google.com
PING google.com (172.217.1.78): 56 data bytes
64 bytes from 172.217.1.78: icmp_seq=0 ttl=55 time=13.503 ms
64 bytes from 172.217.1.78: icmp_seq=1 ttl=55 time=13.644 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 13.503/13.573/13.644/0.071 ms
```

Send five pings, then display ping statistics.
```bash
$ ping -c 5 google.com
PING google.com (172.217.6.206): 56 data bytes
64 bytes from 172.217.6.206: icmp_seq=0 ttl=54 time=13.493 ms
64 bytes from 172.217.6.206: icmp_seq=1 ttl=54 time=12.712 ms
64 bytes from 172.217.6.206: icmp_seq=2 ttl=54 time=12.952 ms
64 bytes from 172.217.6.206: icmp_seq=3 ttl=54 time=13.592 ms
64 bytes from 172.217.6.206: icmp_seq=4 ttl=54 time=13.068 ms

--- google.com ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 12.712/13.163/13.592/0.332 ms
```

Ping via a specified network interface: helps answer the question *Is a network interface up?*
```bash
$ ping -I eth0 google.com
64 bytes from 172.217.6.206: icmp_seq=0 ttl=54 time=14.014 ms
64 bytes from 172.217.6.206: icmp_seq=1 ttl=54 time=12.508 ms
...
```

Those are the ones I use most commonly!

### `tcpdump` - View all trafic over the network

```bash
sudo tcpdump port 80 -w capture.pcap
```

A few flags that would be good to know:
- `-X`: Show packet contents in acii / hex (ala hexdump)
- `-n`: tells tcpdump to auto-resolve domain names to ips
- `--nn`: tells tcpdump to auto-resolve ports to their numerical equivelents. For some reason, by default, addresses are like `1.2.3.4.http` when they really mean `1.2.3.4:80`. This flag makes that happen.
- `-s`: The amount of the body to display of the packet.


# Still figuring out myself.....


### `netstat` - Show information network statistics and activity

**NOTE: I haven't figured this one out yet.**

This tool has a ton of flags to change what it displays:
- `-t`: Show tcp requests
- `-u`: Show udp requests

