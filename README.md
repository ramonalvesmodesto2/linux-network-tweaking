# linux-network-tweaking

<b>Customized HYBLA Congestion Control for OPENWRT and Linux distributions + Network Tweaking by: </b><a href="http://ipv4.throttlefix.com">Throttlefix.com</a>

<a href="mailto:michael.gehaa@gmail.com">Contact email</a>

<a href="https://throttlefix.com/forum/">Donate</a>

# OPENWRT

<b> Download OPENWRT Sdk according to your router architecture</b>

<pre>cd sdk</pre>

Upload "hybla_max" folder to package folder
 
<pre>make menuconfig</pre>
 
Select Kernel modules --> Network Support ---> kmod-hybla_max
  
<pre>make package/hybla_max/compile V=99</pre>

Navigate to bin directory, find and install kmod-hybla_max.ipk
 
<pre>insmod tcp_hybla_max.ko</pre>
  
<pre>echo "tcp_hybla_max" > /etc/modules.d/hybla_max</pre>
  
<pre>sysctl -w net.ipv4.tcp_congestion_control=hybla_max</pre>

# Linux

<b>Download "tcp_hybla_max/module" folder</b>

<pre>cd tcp_hybla_max/module/</pre>
  
<pre>make</pre>
  
<pre>insmod tcp_hybla_max.ko</pre>
  
<pre>sysctl -w net.ipv4.tcp_congestion_control=hybla_max</pre>

<b>You can load module on startup in rc.local file</b>



# Route Tweaking with ip route
<b>Tweak your lan network and at first check your network routes because it differs from network to network. ex: 192.168.1.0/24</b>
<pre>ip route</pre>
<pre>
[root@THROTTLEFIX.COM_OpenWrT:/root]#ip route
default via 172.23.0.250 dev eth0.2 proto static src 172.23.0.249
172.23.0.0/16 dev eth0.2 proto kernel scope link src 172.23.0.249
172.23.0.250 dev eth0.2 proto static scope link src 172.23.0.249
192.168.1.0/24 dev br-lan proto kernel scope link src 192.168.1.1
[root@THROTTLEFIX.COM_OpenWrT:/root]#
</pre>
Create .sh script in order not to get disconnect because we need to delete route and re-add a new one
<pre>vi run.sh</pre>
add the following :
<pre>ip route del 192.168.1.0/24 dev br-lan proto kernel scope link src 192.168.1.1</pre>
<pre>ip route add 192.168.1.0/24 dev br-lan ttl-propagate enabled proto dhcp  scope global window 999999999  rtt 4000000000ms rttvar 0ms cwnd 1 initcwnd 1000 nexthop dev br-lan weight 1 realm 2</pre>
<pre>sh run.sh</pre>

<b>Note: ttl-propagate option is not available on old iproute, please make sure you are running on latest kernel</b>

# Sysctl.conf Tweaks
<pre>
net.ipv4.tcp_congestion_control=hybla_max
net.ipv4.tcp_fastopen = 3
kernel.panic=3
vm.swappiness=89
net.ipv4.conf.default.arp_ignore=1
net.ipv4.conf.all.arp_ignore=1
net.ipv4.ip_forward=1
net.ipv4.icmp_echo_ignore_broadcasts=1
net.ipv4.icmp_ignore_bogus_error_responses=1
net.ipv4.igmp_max_memberships=100
net.ipv4.tcp_ecn=0
net.ipv4.tcp_fin_timeout=10
net.ipv4.tcp_keepalive_time=60
net.ipv4.tcp_keepalive_intvl=2
net.ipv4.tcp_syncookies=1
net.ipv4.tcp_timestamps=1
net.ipv4.tcp_sack=1
net.ipv4.tcp_dsack=1
fs.file-max = 512000
net.ipv6.conf.default.forwarding=1
net.ipv6.conf.all.forwarding=1
vm.overcommit_memory = 2
vm.overcommit_ratio  = 100
net.netfilter.nf_conntrack_acct=1
net.netfilter.nf_conntrack_checksum=0
net.netfilter.nf_conntrack_max=1638400
net.netfilter.nf_conntrack_tcp_timeout_established=7440
net.netfilter.nf_conntrack_udp_timeout=60
net.netfilter.nf_conntrack_udp_timeout_stream=180
</pre>
