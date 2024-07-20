

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


Property                Example Server 1                Example Server 2

interface               192.168.1.21	                192.168.1.22

unicast_src_ip          192.168.1.22                    192.168.1.21

virtual_ipaddress       192.168.1.20/24                 192.168.1.20/24


auth_pass               P@$$w05d                        P@$$w05d



systemctl enable --now keepalived.service
systemctl status keepalived.service
