# linux-network-tweaking
HYBLA MAX Congestion Control - Openwrt &amp; Linux

Customized Congestion Control for OPENWRT and Linux distributions + Network Tweaking


///////////////////////////////////////////////////////////
# OPENWRT

Download OPEN WRT Sdk according to your router architecture

# cd sdk
Upload "hybla_max" folder to package folder
# make menuconfig
Select Kernel modules --> Network Support ---> kmod-hybla_max
# make package/hybla_max/compile V=99


Navigate to bin directory, find and install kmod-hybla_max.ipk
# insmod tcp_hybla_max

# echo "tcp_hybla_max" > /etc/modules.d/hybla_max


# sysctl -w net.ipv4.tcp_congestion_control=hybla_max

///////////////////////////////////////////////////////////


# Linux

Download "tcp_hybla_max/module" folder

# cd tcp_hybla_max/module/
# make
# insmod tcp_hybla_max.ko
# sysctl -w net.ipv4.tcp_congestion_control=hybla_max


You can load module on startup in rc.local file



////////////////////////////////////////////////////////////
