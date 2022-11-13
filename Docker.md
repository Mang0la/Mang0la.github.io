# CYB-3353-01-22-FA-Docker-Installation-Guide (PiHole Route)
A short installation guide with documentation for installing Docker and PiHole via Docker

## Installing Docker
https://docs.docker.com/desktop/install/windows-install/
~~https://docs.docker.com/engine/install/ubuntu/
1. I used the instructions linked in ubuntu to install Docker

2. I accepted the default values for most things from the prompts

3. `sudo apt-get remove docker docker-engine docker.io containerd runc` - to uninstall any older version of Docker existing on the system

4. `sudo apt-get update` & `sudo apt-get install \ ca-certificates \ curl \ gnupg \ lsb-release` - updates the apt package and allows `apt` to use over HTTPS respectively

5. `sudo mkdir -p /etc/apt/keyrings` & `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg` - adds Docker's official GPG key

6. `echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null` - sets up the repository

6. `sudo apt-get update` - updates the apt package

7. `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin` - installs Docker

8. `sudo docker run hello-world` - verifies the install by running the hello world image~~


 I used the instructions linked above to install 'Docker Desktop Installer.exe'

## Wordpress Server
