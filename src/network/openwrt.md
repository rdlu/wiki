# OpenWRT tricks

## Blocking URLs at dnsmasq

```sh
uci add_list dhcp.@dnsmasq[0].address='/address.com/127.0.0.1'
uci commit dhcp
/etc/init.d/dnsmasq restart
```
