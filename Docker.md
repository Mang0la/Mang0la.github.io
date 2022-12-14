# CYB-3353-01-22-FA-Docker-Installation-Guide (PiHole Route)
A short installation guide with documentation for installing Docker and PiHole via Docker

## Installing Docker
~~https://docs.docker.com/engine/install/ubuntu/~~
~~1. I used the instructions linked in ubuntu to install Docker~~

~~2. I accepted the default values for most things from the prompts~~

~~3. `sudo apt-get remove docker docker-engine docker.io containerd runc` - to uninstall any older version of Docker existing on the system~~

~~4. `sudo apt-get update` & `sudo apt-get install \ ca-certificates \ curl \ gnupg \ lsb-release` - updates the apt package and allows `apt` to use over HTTPS respectively~~~~

~~5. `sudo mkdir -p /etc/apt/keyrings` & `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg` - adds Docker's official GPG key~~

~~6. `echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null` - sets up the repository~~

~~6. `sudo apt-get update` - updates the apt package~~

~~7. `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin` - installs Docker~~

~~8. `sudo docker run hello-world` - verifies the install by running the hello world image~~
> I have stopped my attempt to install using ubuntu through PuTTY as I've been constantly running into issues simply installing packages or using sudo and have opted to use the installer wizard for Windows

https://docs.docker.com/desktop/install/windows-install/
 I used the instructions linked above to install 'Docker Desktop Installer.exe'
 1. I clicked on 'Download Docker Desktop for Windows'
 2. ![image](https://user-images.githubusercontent.com/56270888/201550454-4844c439-d55e-4bba-a493-335912df855c.png)
 3. I accepted the default settings and opened Docker

## Wordpress Server
1. I opened Command Prompt to begin running commands
2. `docker compose -version` - to make sure what version of Docker I'm running
3. `mkdir wordpress` - makes a directory for wordpress
4. `cd wordpress` - moves to the neew directory
5. `echo "something" > docker-compose.yml` - I echoed something to create a new file in the directory
6. I then navigated to the directory using File Explorer and opened it with VScode (though you can use any other text editor)
7. I pasted:

![image](https://user-images.githubusercontent.com/56270888/201554608-4b289eec-7176-4f98-9fb5-baa1ad7e3677.png)

  
  - inside the file
8. `docker compose up -d` - starts the containers for wordpress
9. `http://localhost:8000/` - I typed this into my browser URL and was met with the WordPress screen
10. Create an account by inputting the relevant information that you're prompted
11. ![image](https://user-images.githubusercontent.com/56270888/201551298-8395cf4a-b8c9-42a4-a8df-c0bfb676d41c.png)
12. Now you're done!

