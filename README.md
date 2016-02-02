# openconnectd
small daemon to control vpn connections via [OpenConnect](http://www.infradead.org/openconnect/) without need of root privileges

## requrements
1. latest [OpenConnect] (http://git.infradead.org/users/dwmw2/openconnect.git) built from source with patches from `openconnect-patches` applied (at least until they are included)
2. `incron` package installed

## installation
copy `etc` and `usr` contents to the root of your filesystem

## configuration
create file describing your connection somewhere.
it is and OpenConnect config file (see `--config`), should be named like `myconnection.vpn` (`.vpn` ending is mandatory) and should contain options necessary for `--authenticate`, e.g.:
```shell
authgroup=CiscoGroup
cafile=../../certificates/cert151.pem
certificate=../../certificates/CiscoVPN.crt.pem
no-cert-check
sslkey=../../certificates/CiscoVPN.key.pem
user=CiscoUser
host=62.52.42.32:34443

password=MyCiscoPassword
```
please notice `password` option -- its support added by script for your convinience

if you need to pass any options for tunnel, create config file with same name in `/etc/openconnectd/`, e.g. `/etc/openconnectd/myconnection.vpn`:
```bash
mtu=1400
reconnect-timeout=15
```
## usage
run `vpn-connect myconnection` and wait for a few seconds. it will authenticate, establish a tunnel and create a file `myconnection.info` in `/tmp/openconnectd`.
you can specify multiple connections at the same time: `vpn-connect myconnection myanotherconnection myonemoreconnection`.
to disconnect run `vpn-disconnect myconnection myanotherconnection myonemoreconnection`, it will check for available tunnels and shut them down

## disclaimer
I made this tool by myself and for myself. just to simplify my workflow. but it would be great if it will be useful for someone else
