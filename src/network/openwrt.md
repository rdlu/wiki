# OpenWRT tricks

## TIM Live Brazil Fiber (FTTH)

It uses PPPoE like the VDSL, but you need to set the VLAN.

Unlike the VDSL connections, for FTTH connections you need the VLAN ID 100.

Unfortunately you can't use the Luci GUI interface. Tested with OpenWrt 21.02.

Edit `/etc/config/network`, specifically the find `wan` and `wan6` interfaces:

```
config interface 'wan'
        option proto 'pppoe'
        option password 'guest'
        option ipv6 'auto'
        option username 'guest'
        option device 'eth0.2'
        option ifname 'dsl0.100'  ## This is essential

config interface 'wan6'
        option proto 'dhcpv6'
        option device 'eth0.2'
```

IPv6 needs more work, since my connection does not have it enabled, trying to enable inside the `wan6` prevents me the connection with PADO timeouts.

Be aware that `device 'eth0.2'` can be different for your router model.

## Slow PPPoE

If you're having slow throughput using PPPoE, specially under the FTTH connection above, enable `Software flow offloading` inside `Firewall`  settings.

It will make QoS SQM to not work, but since I have plenty bandwith now, I just disabled it.

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