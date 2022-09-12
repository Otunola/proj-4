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
13. <img width="835" alt="Screen Shot 2022-09-12 at 1 38 00 AM" src="https://user-images.githubusercontent.com/112595648/189736216-0208e9ec-dcc2-4600-9420-77b08234f20c.png">

14. add a file name to the book directory named server : touch server.js
15. paste the code below to the server.js file with Vim : vi server.js
16. var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

16. since mongoDB is already istalled,we now Install Express and set up routes to the server and also install mongooose package to provide a striaght-forward schema-based solution to your application data.
17. install mongoose with the command : sudo npm install express mongoose
18. Then in books folder create a folder named apps and cd into it with the command : mkdir apps && cd apps
19. create a file named route.js with command : touch route.js
20. paste the code below to the route.js file created with the command : vi route.js
21. var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};

22. Create a folder named models in the app folder and cd into it with the command : mkdir models && cd models
23. create a file called book.js and paste the code below into it with the command : vi book.js
24. 
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);

24. We then access the routes with AngularJS. AngularJS provides a web framework for creating dynamic views in your web applications. we use AngularJS to connect our web page with Express and perform actions on our book register.
25. Now change back to Books directory : cd ../..
26. create a directorynamed public and cd into it : mkdir public && cd public
27. create a file named script.js and paste the following script in to it with the command : vi script.js
28. var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});

29. Create a file named index.html in the public folder, then copy and paste the code below in to the index.html with the command : vi indexx.html
30. 
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>

29. Change the directory back into Books with : cd ..
30. start the server by running the commnd : node server.js
31. <img width="765" alt="Screen Shot 2022-09-12 at 1 55 30 AM" src="https://user-images.githubusercontent.com/112595648/189743798-e4a7321e-e284-4051-8e04-c931f7de06f2.png">
32. From the image above, the server is up and running and we can connect to it via port 3300. but then we need to open TCP port 3300 in your AWS console as shown in the image below
33. ![Screen Shot 2022-09-12 at 1 56 03 AM](https://user-images.githubusercontent.com/112595648/189744269-62fb016b-0dcf-48a6-951d-262f1b12d9ea.png)
34. now open ur instance ip address slash 3300 on the a new tab and below should be seen
35. ![Screen Shot 2022-09-12 at 1 57 35 AM](https://user-images.githubusercontent.com/112595648/189744708-3859c659-bcde-4aff-a2cf-8c41419fd79a.png)
