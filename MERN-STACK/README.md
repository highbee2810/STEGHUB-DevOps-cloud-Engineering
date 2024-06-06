# Project Documentation: MERN Stack on EC2 Instance

## Overview

This project is a documentation guide for setting up a MERN (MongoDb,ExpressJs,ReactJs,NodeJs) stack on an Amazon EC2 instance. The MERN stack is a popular web development environment that provides the necessary components to run dynamic websites and web applications using Javascript technologies.

### Components

- **MongoDB:** a no-sql database that store data inform of documents
- **ExpressJs:** a serverside web application javascript framework
- **ReactJs:** a frontend Javascript framework used for UI(user interface) design.
- **NodeJs:** a runtime environment that allows javascript to run on machine instead of browsers.

## Setting up the EC2 Instance

1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
![Screenshot (128)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/60d74685-0cad-40de-be76-485b10068ec0)


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
ssh -i "steghub.pem" ubuntu@18.209.18.61
```
![Screenshot (99)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/81da729e-cc44-4fe4-88da-335c6c1729fc)

## Installing MERN Stack

1. **Update Package Repository**:
   ```bash
   sudo apt update
   ```
   ![Screenshot (131)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1bc083cd-f68c-436f-999b-503fb23421fa)
2. **Upgrade ubuntu**:
   ```
   sudo apt upgrade
   ```
3.**Get the location of NodeJs from the ubuntu  Repository**:
```
curl fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![Screenshot (132)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/50f890e3-c16a-4b15-9ab3-f5bf6f9361c2)

Purpose of the Script:
The NodeSource setup script performs several tasks:

Adds the NodeSource repository to your system's package manager sources.
Installs the required GPG key for the repository.
Updates the package lists.
Ensures your system is ready to install Node.js 18.x via your package manager (apt on Debian-based systems like Ubuntu).
3.**Install  NodeJs on the server**:
```
sudo apt-get install nodejs -y
```
![Screenshot (133)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a8c3f421-6fac-47d8-931f-ddc07a460d26)
Note: the above command installs both node.js and npm. NPM is a package manager for Node just as apt is a package manager for Ubuntu. It is used to install Node modules and packages and to manage dependency conflicts.
4.**verify the installation**:
```
node -v        // Gives the node version

npm -v        // Gives the node package manager version
```
![Screenshot (134)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/dc730981-41a9-4c6f-951f-d35eb390aa16)
## Application code setup
1**Create a new directory for your To-Do project**:
```
mkdir Todo
```
2.**confirm it has been created**:
```
ls
```
3.**enter into the directory**
```
cd Todo
```
4.**Use command npm init to initialize your project in the directory**:
```
npm init
```
![Screenshot (135)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/0b9eb955-29ec-44be-8b8e-fc1c82a8c341)
## Install ExpressJs
ExpressJs is a framework for Nodejs therefore a lot of things developers we need to programmed have been taking care of.
1**Install expressjs using npm**:
```
npm install express
```
![Screenshot (136)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e51c625d-bd13-4f75-82f3-1b88862ad1f2)
2**Create a file index.js**:
```
touch index.js
```
3.**Run ls to confirm the file is created**:
```
ls
```
4.**Install the dotenv module**:
```
npm install dotenv
```
![Screenshot (137)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6ed2e62c-0c5e-4cc6-b188-542ac843bdab)
5.**Open the index.js file**:
```
vim index.js
```
![Screenshot (138)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/28de8014-a058-44b3-9401-c55d3281a238)
6.**Copy and paste the code below into it and save it**:
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
![Screenshot (139)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/80e637cf-66b6-4f3f-b29e-a14349ee31fd)
7.**it is time so start our server and see how it works**:
```
node index.js
```
![Screenshot (140)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/39061480-f609-479e-8599-c5296938ccce)
if everything goes well you should see server running on port 5000
8.**Now we need to edit our inbound rule for instance to allow traffic through port 5000**:
![Screenshot (141)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6615f1d2-5c5a-417b-94f6-af47deacf163)
9.**Now open your browser and try access your server follow by port 5000**:
```
http://<public-ip-address>:5000
```
![Screenshot (142)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/706ff28e-7542-4f1a-b125-596393cc2f4c)
## Routes
There are three actions that the ToDo application needs to be able to do:

Create a new task
Display list of all task
Delete a completed task
Each task was associated with some particular endpoint and used different standard HTTP request methods: POST, GET, DELETE.

For each task, routes were created which defined various endpoints that the ToDo app depends on.
1.**Create a folder Routes**:
```
mkdir Routes
```
2.enter the folder
```
cd Routes
```
3.**Create a file api.js**:
```
touch api.js
```
4.**Open the file and Copy and paste the code below in it**:
```
vim api.js
```
the code to copy
```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

});

module.exports = router;
```
![Screenshot (143)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/277353b9-88aa-4022-a083-3c2460647693)
## MODELS
The model in a JavaScript application is crucial for managing and encapsulating the data and business logic. It serves as the central component that the rest of the application interacts with, ensuring that data flows correctly and consistently throughout the system. Whether in a frontend, backend, or full-stack context, the model plays a key role in the application's architecture.

1.**Change directory to Todo and Install mongoose a nodejs package that makes working with mongodb easier**:
```
npm install mongoose
```
![Screenshot (145)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8748b364-3a46-4050-af8e-6ce367c1fbfe)


2.**Create a new folder models and change into it**:
```
mkdir models
cd models
```
3.**Create a new file todo.js and open it and copy and paste the following code into it**:
```
touch todo.js

vim todo.js
```
code to copy into it 
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
  }
});

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
![Screenshot (146)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/68894f71-6059-480b-abae-c07908696010)
4.**Now we need to update the routes from the file api.js in the 'routes' directory**:
change to the directory routes and open the file api.js
delete the code inside the file , copy and paste the code below into it
```
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {
  // This will return all the data, exposing only the id and action field to the client
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
  } else {
    res.json({
      error: "The input field is empty"
    });
  }
});

router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({"_id": req.params.id})
    .then(data => res.json(data))
    .catch(next);
});

module.exports = router;
```
![Screenshot (147)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/25a55567-787e-4dd5-b810-db93f4864229)
## MongoDB database
mLab is a popular cloud database service that provided managed MongoDB hosting. It is known for its ease of use, robust features, and support for developers looking to deploy and manage MongoDB databases without worrying about the underlying infrastructure. However, mLab was acquired by MongoDB Inc., and its services were integrated into MongoDB Atlas.
we will sign up an account.
![Screenshot (148)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3c3befdb-1c90-4e2c-a4f8-a033ce617e6d)
create a cluster with AWS as a provider and select a region close to youy
![Screenshot (150)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/05310dcd-2fbf-4b9f-93b9-75084166f018)
create a Database and collection
![Screenshot (151)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/9c142903-e911-4b7f-8b33-dec51f876036)

**Create a file in your Todo directory and name it .env, open the file**:
```
touch .env
vim .env
```
Add connection string to connect to MongoDB database
```
DB = ‘mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority’
```
![Screenshot (153)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/432bb65c-6a6f-4676-892c-09c73eaf9ae9)
**3. Update the index.js to reflect the use of .env so that Node.js can connect to the database.**
```
vim index.js
```
delete existing code and copy the below code
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
  .catch(err => console.log(err));

// Since mongoose promise is deprecated, we override it with Node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
  console.log(err);
  next();
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

![Screenshot (154)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/0fc2f52a-2840-4bd7-8f1a-b3222d4b1d3e)


**start your server**
```
node index.js
```


## Testing Backend code without frontend using RESTful API

















