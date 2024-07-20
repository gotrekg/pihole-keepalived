

Dependencies

apt update && apt install sqlite3 sudo git rsync ssh



Install Gravity Sync on Primary 

export GS_INSTALL=primary && curl -sSL https://gravity.vmstan.com | bash

Install Gravity Sync on Secundary 

export GS_INSTALL=secondary && curl -sSL https://gravity.vmstan.com | bash 


Verify connectivity on Secundary Pihole     

./gravity-sync.sh compare

Finally, enable automatic synchronisation by running:

./gravity-sync.sh automate



Configure IP failover


sudo apt install keepalived libipset13 -y


sudo mkdir /etc/scripts
sudo sh -c "curl https://pastebin.com/raw/npw6tcuk | tr -d '\r' > /etc/scripts/chk_ftl"
sudo chmod +x /etc/scripts/chk_ftl


Primary 

sudo curl https://raw.githubusercontent.com/gotrekg/pihole-keepalived/main/keepalived_primary -o /etc/keepalived/keepalived.conf

Secondary 

sudo curl https://raw.githubusercontent.com/gotrekg/pihole-keepalived/main/Keepalived_secondary -o /etc/keepalived/keepalived.conf


We now need to edit the configuration on both servers.


|      Property        | Description                                                 | Example Server 1| Example Server 2|
|----------------------|-------------------------------------------------------------|-----------------|-----------------|
| interface  | The LAN network interface name. Run ip list to view available interfaces if you are unsure.   | eth0            |         eth0   |
| unicast_src_ip  | The IP address of the server you are currently configuring.   | 192.168.1.21             |      192.168.1.22            |
| nicast_src_ip  | The IP address of the other server.   |         192.168.1.22         |       192.168.1.22        |                   
| virtual_ipaddress    | The virtual IP address shared between the 2 servers, provided in CIDR notation. This must be the same on both servers.   |     192.168.1.20/24     |        192.168.1.20/24        |
| auth_pass  | A shared password (max 8 characters). This must be the same on both servers   |       P@$$w05d       |        P@$$w05d       |





systemctl enable --now keepalived.service
systemctl status keepalived.service
