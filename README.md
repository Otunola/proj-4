# proj-4 MEAN STACK DEPLOYMENT TO UBUNTU IN AWS (implement a simple Book Register web form using MEAN stack)
1. The first the to do is to install node.js but before then we have to update and upgrade our ubuntu server with the command: sudo apt update and update with command : sudo apt upgrade
2. Add certificates with the command : sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -   (this is to tell ubuntu to download node.js from here)
3. Then install node.js with the command : sudo apt install -y nodejs
4. check  the version of node.js with the command : node -v
5. Check if the npm is already installed with the command  : npm -v
