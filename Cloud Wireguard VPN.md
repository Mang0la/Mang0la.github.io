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
2. `version: '3.8'
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
      - type: bind
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
      - net.ipv4.conf.all.src_valid_mark=1` - paste this into the docker-compose.yml
