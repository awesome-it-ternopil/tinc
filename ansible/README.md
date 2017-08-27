firewall-cmd --zone=public --add-port=655/tcp  --permanent
yum install tinc -y
mkdir /etc/tinc/{{connection.name}}/hosts
template tinc-up
template tinc-down
template tinc.conf
sudo chmod 755 /etc/tinc/myvpn/tinc-*
tincd -n myvpn -K4096

sudo sed -e 's/#net\.ipv4\.ip_forward=1/net\.ipv4\.ip_forward=1/g' -i /etc/sysctl.conf
sudo sysctl -w net.ipv4.ip_forward=1

firewall-cmd --direct --add-rule ipv4 nat POSTROUTING 0 -o eth_ext -j MASQUERADE
firewall-cmd --direct --add-rule ipv4 filter FORWARD 0 -i eth_int -o eth_ext -j ACCEPT
firewall-cmd --direct --add-rule ipv4 filter FORWARD 0 -i eth_ext -o eth_int -m state --state RELATED,ESTABLISHED -j ACCEPT


rsync hosts
rsync -a nadmin@193.169.81.251:/etc/tinc/vpn1/hosts .
rsync -a nadmin@86.111.189.6:/etc/tinc/vpn1/hosts .
rsync -a nadmin@193.169.81.251:/etc/tinc/vpn1/tinc.conf ../.

chmod +x /etc/tinc/myvpn/tinc-up
chmod +x /etc/tinc/myvpn/tinc-down
# Enable and start tinc:
systemctl enable tinc@myvpn
systemctl start tinc@myvpn

add string with new host to all conf


https://community.ubnt.com/t5/EdgeMAX/Multisite-VPN-using-Edgerouter/m-p/1301568#M72109
# tinc
