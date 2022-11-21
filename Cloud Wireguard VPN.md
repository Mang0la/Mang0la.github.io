# CYB-3353-01-22-FA-Cloud-Wireguard-VPN-Guide
A short installation guide of installing Wireguard and testing a VPN connection

## Digital Ocean
### Resources: 
https://m.do.co/c/4d7f4ff9cfe4 
https://thematrix.dev/setup-wireguard-vpn-server-with-docker/ 

1. Using the link above I received $200 credit for two months to use when signing up and relied on the free trial
2. I created a Droplet with the $4/month plan, Ubuntu 20.04, Basic, and a Regular Intel CPU with no additional add-ons
3. I chose to use a password to access it and made it as secure as possible
4. I accessed the console of that droplet instance
5. `sudo apt install apt-transport-https ca-certificates curl software-properties-common -y` - installs requirements to set up docker
6. `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -` - added a docker key
7. `sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"` - added a docker repo. This one is specifically for 32/64-bit OS
8. `apt-cache policy docker-ce` - switched to the new repo
9. `sudo apt install docker-ce -y` - installed docker
10. `sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose` - isntalls docker-compose
11. `sudo chmod +x /usr/local/bin/docker-compose` - sets the permission

## Wireguard
### Resource
https://thematrix.dev/setup-wireguard-vpn-server-with-docker/ 
1. `mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml` - ran these commands on the new server to setup the docker-compose
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - ty
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
   - paste this into the docker-compose.yml
2. paste this into the file. But need to change things based on your information. 
3. `TZ=Asia/Hong_Kong` - must be changed for your timezone, so US=America/Chicago for CST. https://en.wikipedia.org/wiki/List_of_tz_database_time_zones you can find the information here
4. `SERVERURL` - is the server IP address. You can find it on DigitalOcean's dashboard
5. `PEERS` - the number of user-config-files to generate, or the names of user-config-files. If you enter PEERS=3, it will generate peer_1, peer_2 and peer_3. If you enter PEERS=pc1,pc2,phone1, it will generate peer_pc1, peer_pc2 and peer_phone1.
6. Save and exit the file
7. `cd ~/wireguard/`
   `docker-compose up -d` - starts up Wireguard

## Testing the VPN connection
1. `docker-compose logs -f wireguard` - composes logs and has a QR code needed to scan 
2. I downloaded Wireguard on my phone and laptop preemptively.
3. I first went to ipleak.net to find my initial IP address. I  then opened Wireguard on my phone to scan the QR code.
4. I went back to ipleak.net and took a snip of both instances to show the changes.
5. ![image](https://user-images.githubusercontent.com/56270888/202992783-ff0792a3-f15b-4326-b31d-be40425813fd.png)
![image](https://user-images.githubusercontent.com/56270888/202992808-dc501bb6-2ec0-4d29-aed2-5c697b13f0a8.png)
> The before and after images of the VPN connection respectively
6. `cd wireguard/config`, `ls`, `cd peer_pc1` - these steps show me the config files' locations
7. Fromt there I did `cat peer_pc1` - to find the configuration for a PC and copied it into notepad to have it effectively on my hardware.
8. I opened Wireguard on my PC and found the peer_pc1.conf file on my PC and added it as a tunnel. I then activated it and opened another instance of ipleak.net
9. ![image](https://user-images.githubusercontent.com/56270888/202992663-1404d4ce-c499-4eac-baa8-c29bcbb33bca.png)
> The image above shows the resulting changes.
> Please do not forget to deactivate the account to prevent any unnecessary charges.
