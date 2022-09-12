# proj-4 MEAN STACK DEPLOYMENT TO UBUNTU IN AWS (implement a simple Book Register web form using MEAN stack)
1. The first the to do is to install node.js but before then we have to update and upgrade our ubuntu server with the command: sudo apt update and update with command : sudo apt upgrade
2. Add certificates with the command : sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -   (this is to tell ubuntu to download node.js from here)
3. Install MondoDB with the command : sudo apt install -y mongodb
4. start the mongoDB server eith the command : sudo service mongodb start
5. check that the MOngoDB server is up and running properly with the command : sudo systemctl status mongodb

6. Then install node.js with the command : sudo apt install -y nodejs
7. check  the version of node.js with the command : node -v
8. Check if the npm is already installed with the command  : npm -v
9. If nmp -node package manager is not already installed, install with the command : sudo apt install -y npm
10. body-parser package needs to be instaaled to help process json files passed in request to the server. and its installed with the command : sudo npm install body-parser
11. Since the project requires us to mplement a simple Book Register web form using MEAN stack, so we create a directory called books and cd into it with the command : mkdir Books && cd Books
12. In the book directory, initialise the npm project with the command below and answer the prompted questions: npm init
13. add a file name to the book directory named server : touch server.js
14. paste the code below to the server.js file with Vim : vi server.js
15. var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
![image](https://user-images.githubusercontent.com/112595648/189731492-ee2ccf93-e565-4c52-b27e-e899c50fc890.png)



