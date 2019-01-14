install ss-libev

wget --no-check-certificate -O shadowsocks-libev.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh && chmod +x shadowsocks-libev.sh && ./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log

order:

./shadowsocks-libev.sh uninstall

/etc/init.d/shadowsocks-libev start | stop | restart | status

/etc/shadowsocks-libev/config.json



install google bbr

wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh

check:

uname -r



mkdir /root/udpspeeder

cd /root/udpspeeder

wget https://github.com/wangyu-/UDPspeeder/releases/download/20180522.0/speederv2_binaries.tar.gz

tar -zxf speederv2_binaries.tar.gz


mkdir /root/tinymapper

cd /root/tinymapper

wget https://github.com/wangyu-/tinyPortMapper/releases/download/20180224.0/tinymapper_binaries.tar.gz

tar -zxf tinymapper_binaries.tar.gz


mkdir /root/udp2raw

cd /root/udp2raw

wget https://github.com/wangyu-/udp2raw-tunnel/releases/download/20180225.0/udp2raw_binaries.tar.gz

tar -zxf udp2raw_binaries.tar.gz



cd /etc/rc.local

sleep 10

nohup /root/udpspeeder/speederv2_amd64 -s -l 0.0.0.0:8888 -r 127.0.0.1:9999 -f2:4 -k "password" --mode 0 -q1 &

sleep 10

nohup /root/udp2raw/udp2raw_amd64 -s -l 0.0.0.0:7777 -r 127.0.0.1:8888 -k "password" --raw-mode faketcp &

sleep 10

nohup /root/tinymapper/tinymapper_amd64 -l 0.0.0.0:8888 -r 127.0.0.1:9999 -t &



shutdown -r now

killall speederv2_amd64


udp2raw.exe -c -l 127.0.0.1:8888 -r serverIp:7777 -k "password" --raw-mode easyfaketcp

has udp2raw:

speederv2.exe -c -l 0.0.0.0:7777 -r 127.0.0.1:8888 -f2:4 -k "password" --mode 0 -q1

no has udp2raw：

speederv2.exe -c -l 0.0.0.0:7777 -r serverIp:8888 -f2:4 -k "password" --mode 0 -q1

tinymapper.exe -l 0.0.0.0:7777 -r serverIp:8888 -t
