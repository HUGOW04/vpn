# vpn

#Compile
```
make
```

# generate vpn.key
```
dd if=/dev/urandom of=vpn.key count=1 bs=32
```

# print vpn.key
```
base64 < vpn.key
echo 'HK940OkWcFqSmZXnCQ1w6jhQMZm0fZoEhQOOpzJ/l3w=' | base64 --decode > vpn.key
```

# Example on how to use server
```
sudo ./vpn server vpn.key auto 1959
```

# Example on how to use client
```
sudo ./vpn client vpn.key 34.216.127.34 1959
```

You are connected. Just hit `Ctrl`-`C` to disconnect.


## A note on DNS

If you were previously using a DNS resolver only accessible from the local network, it won't be accessible through the VPN. That might be the only thing you may have to change. Use a public resolver, a local resolver, or DNSCrypt.

Or send a pull request implementing the required commands to change and revert the DNS settings, or redirect DNS queries to another resolver, for all supported operating systems.

## Advanced configuration

```text
vpn   "server"
        <key file>
        <vpn server ip or name>|"auto"
        <vpn server port>|"auto"
        <tun interface>|"auto"
        <local tun ip>|"auto"
        <remote tun ip>"auto"
        <external ip>|"auto"

vpn   "client"
        <key file>
        <vpn server ip or name>
        <vpn server port>|"auto"
        <tun interface>|"auto"
        <local tun ip>|"auto"
        <remote tun ip>|"auto"
        <gateway ip>|"auto"
```

* `server`|`client`: use `server` on the server, and `client` on clients.
* `<key file>`: path to the file with the secret key (e.g. `vpn.key`).
* `<vpn server ip or name>`: on the client, it should be the IP address or the hostname of the server. On the server, it doesn't matter, so you can just use `auto`.
* `<vpn server port>`: the TCP port to listen to/connect to for the VPN. Use 443 or anything else. `auto` will use `443`.
* `<tun interface>`: this is the name of the VPN interface. On Linux, you can set it to anything. Or macOS, it has to follow a more boring pattern. If you feel lazy, just use `auto` here.
* `<local tun ip>`: local IP address of the tunnel. Use any private IP address that you don't use here.
* `<remote tun ip>`: remote IP address of the tunnel. See above. The local and remote tunnel IPs must the same on the client and on the server, just reversed. For some reason, I tend to pick `192.168.192.254` for the server, and `192.168.192.1` for the client. These values will be used if you put `auto` for the local and remote tunnel IPs.
* `<external ip>` (server only): the external IP address of the server. Can be left to `"auto"`.
* `<gateway ip>` (client only): the internal router IP address. The first line printed by `netstat -rn` will tell you (`gateway`).

If all the remaining parameters of a command would be `auto`, they don't have to be specified.


