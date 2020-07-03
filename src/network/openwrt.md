# OpenWRT tricks

## Blocking URLs at dnsmasq

```sh
uci add_list dhcp.@dnsmasq[0].address='/address.com/127.0.0.1'
uci commit dhcp
/etc/init.d/dnsmasq restart
```


## Some useful packages

```
ca-bundle curl ddns-scripts ddns-scripts_cloudflare.com-v4 etherwake flent-tools ip-tiny iptables-mod-conntrack-extra iptables-mod-ipopt kmod-ifb kmod-ipt-conntrack-extra kmod-ipt-ipopt kmod-ipt-raw kmod-sched-cake kmod-sched-core kmod-udptunnel4 kmod-udptunnel6 kmod-wireguard libcap libcurl4 libelf1 libmbedtls12 libmnl0 libncurses6 libqrencode librt luci-app-ddns luci-app-sqm luci-app-wireguard luci-app-wol luci-compat luci-lib-ipkg luci-proto-wireguard nano netperf qrencode speedtest-netperf sqm-scripts tc terminfo wireguard wireguard-tools zlib
```