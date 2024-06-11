# Project Documentation: MEAN Stack on EC2 Instance

## Overview

This project is a documentation guide for setting up a MEAN (MongoDb,ExpressJs,Angular.js,NodeJs) stack on an Amazon EC2 instance. The MEAN stack is a popular web development environment that provides the necessary components to run dynamic websites and web applications using Javascript technologies.

### Components

- **MongoDB:** a no-sql database that store data inform of documents
- **ExpressJs:** a serverside web application javascript framework
- **Angular:** a frontend Javascript framework used for UI(user interface) design.
- **NodeJs:** a runtime environment that allows javascript to run on machine instead of browsers.

## Setting up the EC2 Instance

1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
![Screenshot (183)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/93548b6e-0886-4664-927a-57d27da9721d)


2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.

3. **Add Storage**:
   - Allocate storage space according to your needs.

4. **Add Tags**:
   - Optionally, add tags for better organization.
5. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address
   ![Screenshot (104)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/9aed6d66-a25b-4b3d-85f3-fdeae7b2a53e)

6. **Review and Launch**:
   - Review the configuration and launch the instance.
![Screenshot (129)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/2abfbad5-d0ce-4d8b-83e9-1a69ab39629a)


    
7. **Connect to the Instance**:
   - Use windows terminal to connect to the instance via SSH.
8. **SSH to the instance**
```
ssh -i "steghub.pem" ubuntu@16.170.143.75
```
![Screenshot (184)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/fe8c488e-87f2-4ce1-9636-f7bf911a9d00)


## Installing MEAN Stack
**In this project we are going to implement a simple book register**
## Step 1: Install nodejs
1. **Update Package Repository**:
   ```bash
   sudo apt update
   ```
   ![Screenshot (131)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1bc083cd-f68c-436f-999b-503fb23421fa)
   
2. **Upgrade ubuntu**:
   ```
   sudo apt upgrade
   ```
3. **Add certificates**
   ```
   sudo apt -y install curl dirmngr apt-transport-https lsb-release cacertificates
   curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
   ```
   ![Screenshot (185)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5eee544d-1f97-41b6-a4f6-10aa565ebb1e)

4. **Install nodejs**
   ```
   sudo apt install -y nodejs
   ```
   ![Screenshot (186)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5b35d4e8-c2b3-4aa5-a012-0e215575f595)
## Step 2: Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to
MongoDB that contain book name, isbn number, author, and number of pages.

To import the MongoDB public GPG key, run the following command:
```
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
**By running the command below, you ensure that your system can retrieve and install MongoDB packages from the official MongoDB repository.**

```
echo "deb [arch=amd64] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
**Update package and Install MongoDB**
```
sudo apt-get update
sudo apt install -y mongodb
```
![Screenshot (188)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3f8a8757-fdaa-4a3e-a085-87debdbfd615)
**Start the server**
```
sudo systemctl start mongod
```
**Enable mongodb**
```
sudo systemctl enable mongod
```
**Check the status of mongodb**
```
sudo systemctl status mongod
```
![Screenshot (189)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/19e0c4c7-839e-4a7d-9204-010436d78ba3)

**Install [npm](https://www.npmjs.com) - Node package manager**
```
sudo apt install -y npm
```

**Install 'body-parser package**
We need 'body-parser' package to help us process JSON files passed in
requests to the server.

```
sudo npm install body-parser
```
**Create a folder named 'Books'**
```
mkdir Books
```
change directory to the Books folder
```
cd Books
```
**In the Books directory, Initialize npm project**
```
npm init
```
![Screenshot (190)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/88830cd5-4215-487f-88bf-00b071fa28c7)

**Add and open a file to it named server.js**
```
touch server.js && vim server.js
```
**Copy and paste the web server code below into the server.js file**
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose'); // Make sure mongoose is installed and required
const path = require('path'); // To handle static file serving
const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.error('MongoDB connection error:', err));

// Middleware
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, 'public')));

// Routes
require('./apps/routes')(app);

// Start the server
app.set('port', 3300);
app.listen(app.get('port'), () => {
  console.log('Server up: http://localhost:' + app.get('port'));
});
```
![Screenshot (202)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/4367a778-a4ce-4bea-90c3-fd26cfd849cb)


## Step 3: Install Express and set up routes to the server
Express will be used to pass book information to and from our MongoDB database.
**Install express and mongoose**
```
sudo npm install express mongoose
```
![Screenshot (192)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f5c8f5ed-6cd8-4675-af0a-f774bce0aa5b)

In 'Books' folder, create a folder named apps
```
mkdir apps
```
enter the apps folder
```
cd apps
```
**Create and open a file named routes.js**
```
vi routes.js
```
copy and paste the following code into it
```
const Book = require('./models/book');
const path = require('path');

module.exports = function(app) {
  // Get all books
  app.get('/book', async (req, res) => {
    try {
      const books = await Book.find({});
      res.json(books);
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Add a new book
  app.post('/book', async (req, res) => {
    try {
      const book = new Book({
        name: req.body.name,
        isbn: req.body.isbn,
        author: req.body.author,
        pages: req.body.pages
      });
      const result = await book.save();
      res.json({
        message: "Successfully added book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Update a book
  app.put('/book/:isbn', async (req, res) => {
    try {
      const updatedBook = await Book.findOneAndUpdate(
        { isbn: req.params.isbn },
        req.body,
        { new: true }
      );
      if (!updatedBook) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully updated the book",
        book: updatedBook
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Delete a book
  app.delete('/book/:isbn', async (req, res) => {
    try {
      const result = await Book.findOneAndRemove({ isbn: req.params.isbn });
      if (!result) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Serve static files
  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../public', 'index.html'));
  });
};
```
![Screenshot (193)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/4019ee75-10b1-4cce-8c4c-718d051ffdfd)

In the 'apps' folder, create a folder named models
```
mkdir models
```
enter the models folder
```
cd models
```
Create and open a file named book.js
![Screenshot (195)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5b1e1a59-0454-4d5e-9341-cb485d1d891a)

```
vi book.js
```
copy the code below into it
```
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  name: { type: String, required: true },
  isbn: { type: String, required: true, unique: true },
  author: { type: String, required: true },
  pages: { type: Number, required: true }
});

module.exports = mongoose.model('Book', bookSchema);
```
![Screenshot (194)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b3646cd6-7001-4e75-ad1b-5b2501bf2113)

## Step 4 - Access the routes with AngularJS
AngularJS will be used to connect our web page with Express and perform actions on our book register
**Change directory to the books**
```
cd ../..
```
**Create and open a folder named public**
```
mkdir public && cd public
```
**Add and open a file named script.js**
```
vi script.js
```
Copy and paste the Code below (controller configuration defined) into the
script.js file.
```
var app = angular.module('myApp', []);

app.controller('myCtrl', function($scope, $http) {
  // Get all books
  function getAllBooks() {
    $http({
      method: 'GET',
      url: '/book'
    }).then(function successCallback(response) {
      $scope.books = response.data;
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  }

  // Initial load of books
  getAllBooks();

  // Add a new book
  $scope.add_book = function() {
    var body = {
      name: $scope.Name,
      isbn: $scope.Isbn,
      author: $scope.Author,
      pages: $scope.Pages
    };
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
      // Clear the input fields
      $scope.Name = '';
      $scope.Isbn = '';
      $scope.Author = '';
      $scope.Pages = '';
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Update a book
  $scope.update_book = function(book) {
    var body = {
      name: book.name,
      isbn: book.isbn,
      author: book.author,
      pages: book.pages
    };
    $http({
      method: 'PUT',
      url: '/book/' + book.isbn,
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Delete a book
  $scope.delete_book = function(isbn) {
    $http({
      method: 'DELETE',
      url: '/book/' + isbn
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };
});
```
![Screenshot (196)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a1f132a5-da67-416e-b936-54bd3e089ecb)
**In 'public' folder, create and open a file named index.html**
```
vi index.html
```
Copy and paste the code below in it
```
<!DOCTYPE html>
<html ng-app="myApp" ng-controller="myCtrl">
<head>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
  <script src="script.js"></script>
  <style>
    /* Add your custom CSS styles here */
  </style>
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
    <div ng-if="successMessage">{{ successMessage }}</div>
    <div ng-if="errorMessage">{{ errorMessage }}</div>
  </div>
  <hr>
  <div>
    <table>
      <tr>
        <th>Name</th>
        <th>Isbn</th>
        <th>Author</th>
        <th>Page</th>
        <th>Action</th>
      </tr>
      <tr ng-repeat="book in books">
        <td>{{ book.name }}</td>
        <td>{{ book.isbn }}</td>
        <td>{{ book.author }}</td>
        <td>{{ book.pages }}</td>
        <td><button ng-click="del_book(book)">Delete</button></td>
      </tr>
    </table>
  </div>
</body>
</html>
```
![Screenshot (197)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3b1dd88c-2175-47c1-860a-b009ebc7fe1e)

**Change the directory back to books**
```
cd ..
```
**Start the server by running this command**
```
node server.js
```
![Screenshot (201)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/65a1f8b9-fbf1-4e5c-b1da-160ba0bdddd4)


The server is now up and running, we can connect it via port 3300. You can
launch a separate Putty or SSH console to test what curl command returns locally

**Edit the inbound rule to allow all traffic into port 3300**
![Screenshot (200)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e26b9b15-5e2e-4639-ba9d-56557b9ff17e)

**Open a new tab on your browser and type the ip address enroute port 3300**
```
http://16.170.143.75:3300
```
our applicatoin is live

![Screenshot (199)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f8e01b87-4f2a-4557-a8ba-38fbe0b2ae53)

add books

![Screenshot (203)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/80c37566-eade-4c48-88e2-25f258e56c6e)
## Conclusion
The MEAN stack—comprising MongoDB, Express.js, Angular, and Node.js—provides a powerful and cohesive framework for building dynamic and scalable web applications. By leveraging JavaScript across both the client and server sides, developers can streamline the development process and enhance productivity.







