# Information

TCP provides reliable, ordered, and error-checked delivery of a stream of octets between applications running on hosts communicating by an IP network.

# Concepts:

`CIDR Block`

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
$ 
```

### `netstat` - 

### `ngrep` - search through the content of all tcp requests that touch a machine and search for a phrase inside them.

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


### `tcpdump`
