# Tor_transparent
Tor configs


```sh

Tor proxy instructions


apt-get install tor



service tor start

nano /etc/tor/torrc

VirtualAddrNetwork 10.192.0.0/10
AutomapHostsOnResolve 1
TransPort 9040
DNSPort 5353


service tor restart


#!/bin/sh
# destinations you do not want routed through Tor
NON_TOR="192.168.1.0/24 192.168.0.0/24"
# the UID Tor runs as, change this accordingly for your OS
TOR_UID="43"
# Tor's TransPort
TRANS_PORT="9040"
iptables -F
iptables -t nat -F
iptables -t nat -A OUTPUT -m owner --uid-owner $TOR_UID -j RETURN
iptables -t nat -A OUTPUT -p udp --dport 53 -j REDIRECT --to-ports 5353
for NET in $NON_TOR 127.0.0.0/9 127.128.0.0/10; do
 iptables -t nat -A OUTPUT -d $NET -j RETURN
done
iptables -t nat -A OUTPUT -p tcp --syn -j REDIRECT --to-ports $TRANS_PORT
iptables -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
for NET in $NON_TOR 127.0.0.0/8; do
 iptables -A OUTPUT -d $NET -j ACCEPT
done
iptables -A OUTPUT -m owner --uid-owner $TOR_UID -j ACCEPT
iptables -A OUTPUT -j REJECT

nano transparent_proxy.sh
chmod +x transparent_proxy.sh
./transparent_proxy.sh

curl https://check.torproject.org/ | grep "Congratulations."

```
