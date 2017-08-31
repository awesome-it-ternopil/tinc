For installing new node

Add new node to ssh config file
Update inventory file mesh:
1) Add new node with tinc_ip, public_ip 
2) Generate pair of keyys
```
ssh-keygen -4096 nodename
```
3) Copy generated keys to keys.yml

For delete node from mesh:
1) Remove node from inventory mesh file
2) Delete key from vars/keys.yml

For deactivating crypto  

For using only tcp

For enabing masquerade

For enabing forwarding