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
