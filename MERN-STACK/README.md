# Project Documentation: Deployment of MERN Stack on EC2 Instance

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
   
3. **Upgrade ubuntu**:
   ```
   sudo apt upgrade
   ```
3. **Get the location of NodeJs from the ubuntu  Repository**:
```
curl fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![Screenshot (132)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/50f890e3-c16a-4b15-9ab3-f5bf6f9361c2)

**Purpose of the Script:**
The NodeSource setup script performs several tasks:

Adds the NodeSource repository to your system's package manager sources.
Installs the required GPG key for the repository.
Updates the package lists.
Ensures your system is ready to install Node.js 18.x via your package manager (apt on Debian-based systems like Ubuntu).

4. **Install  NodeJs on the server**:
```
sudo apt-get install nodejs -y
```
![Screenshot (133)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a8c3f421-6fac-47d8-931f-ddc07a460d26)

**Note:** the above command installs both node.js and npm. NPM is a package manager for Node just as apt is a package manager for Ubuntu. It is used to install Node modules and packages and to manage dependency conflicts.

4.**verify the installation**:
```
node -v        // Gives the node version
npm -v        // Gives the node package manager version
```

![Screenshot (134)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/dc730981-41a9-4c6f-951f-d35eb390aa16)
## Application code setup
1. **Create a new directory for your To-Do project**:
```
mkdir Todo
```
2. **confirm it has been created**:
```
ls
```
3. **enter into the directory**
```
cd Todo
```
4. **Use command npm init to initialize your project in the directory**:
```
npm init
```
![Screenshot (135)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/0b9eb955-29ec-44be-8b8e-fc1c82a8c341)

## Install ExpressJs
ExpressJs is a framework for Nodejs therefore a lot of things developers we need to programmed have been taking care of.
1. **Install expressjs using npm**:
```
npm install express
```
![Screenshot (136)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e51c625d-bd13-4f75-82f3-1b88862ad1f2)
2. **Create a file index.js**:
```
touch index.js
```
3. **Run ls to confirm the file is created**:
```
ls
```
4. **Install the dotenv module**:
```
npm install dotenv
```
![Screenshot (137)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6ed2e62c-0c5e-4cc6-b188-542ac843bdab)

5. **Open the index.js file**:
```
vim index.js
```
![Screenshot (138)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/28de8014-a058-44b3-9401-c55d3281a238)

6. **Copy and paste the code below into it and save it**:
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

7. **it is time so start our server and see how it works**:
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
Postman will be used to test the backend code . the endpoints were tested using POST , GET requets 
**Set the header**
**send a post request**
![Screenshot (157)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8b227ec5-6dec-4880-9a4f-d2df45706b1e)
**send a get request**
![Screenshot (158)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/bf4cc539-53e6-41e3-b203-86c7fcc7c629)

## STEP:2 Frontend Creation
**now is time to create the client-side of the application**
in the Todo folder of your application run the code below
```
npx create-react-app client
```
![Screenshot (160)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e4c4b91f-983d-4fee-8653-1adb385bc415)
This created a new folder in the Todo directory called client, where all the react code was added.
## Running a React App
Before testing the react app, the following dependencies needs to be installed in the project root directory.
Install concurrently. It is used to run more than one command simultaneously from the same terminal window.
```
npm install concurrently --save-dev
```
![Screenshot (161)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/843a430d-44a1-4a06-af44-191c3c606405)


**Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.**
```
npm install nodemon --save-dev
```
![Screenshot (162)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/7beebddb-2337-4d7e-8348-921137545775)

In Todo folder open the package.json file, change the highlighted part of the below screenshot and replace with the code below:
```
"scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
}
```
![Screenshot (163)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/57aa4fd0-dbf2-49fe-868d-29e4ae48d768)
## Change proxy package in package.json
change directory
```
cd my-new-app
```
Open the package.json file
```
vim package.json
```
![Screenshot (166)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/edc0e70d-cd4e-4ab0-9b55-33ed18f7a1a0)

Add the key value pair in the package.json file
```
“proxy”: “http://localhost:5000”
```
![Screenshot (167)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1756ff00-bdf2-4734-9a82-f4e3488d2a46)


The whole purpose of adding the proxy configuration above is to make it possible to access the application directly from the browser by simply calling the server url like http://locathost:5000 rather than always including the entire path like http://localhost:5000/api/todos

Ensure you are inside the Todo directory, and simply do:
```
npm run dev
```
![Screenshot (168)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/91c5e912-b121-483f-904a-7f51d70c2a88)


The app opened and started running on localhost:3000
Note: In order to access the application from the internet, TCP port 3000 had been opened on EC2.

## Creating React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For the Todo app, there are two stateful components and one stateless component. From Todo directory, run:
```
cd client
```
change directory to src
```
cd src
```
**2. Inside your src folder, create another folder called “components”**
```
mkdir components
```
![Screenshot (169)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/17f94b36-86ed-4721-b8fb-cd839af9a937)

**3. Inside the ‘components’ directory create three files “Input.js”, “ListTodo.js” and “Todo.js”.**
```
touch Input.js ListTodo.js Todo.js
```
![Screenshot (170)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3f6a999e-2b0e-492c-9d7b-0c0b5d29e7d5)

Open Input.js file
```
vim Input.js
```
Paste in the following:
```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: ""
  }

  handleChange = (event) => {
    this.setState({ action: event.target.value });
  }

  addTodo = () => {
    const task = { action: this.state.action };

    if (task.action && task.action.length > 0) {
      axios.post('/api/todos', task)
        .then(res => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" });
          }
        })
        .catch(err => console.log(err));
    } else {
      console.log('Input field required');
    }
  }

  render() {
    let { action } = this.state;
    return (
      <div>
        <input type="text" onChange={this.handleChange} value={action} />
        <button onClick={this.addTodo}>add todo</button>
      </div>
    );
  }
}

export default Input;
```
![Screenshot (171)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/d6889a13-20a0-4e55-a808-c2e6b8c75e7a)
In oder to make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the client folder
```
cd ../..
```
run the below command to Install Axios
```
npm install axios
```
![Screenshot (172)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/55875981-aaaf-4a87-a52c-88ffd3aea8e7)

Go to components directory
```
cd src/components
```
After that open the ListTodo.js
```
vim ListTodo.js
```
Copy and paste the following code:
```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {
  return (
    <ul>
      {
        todos && todos.length > 0 ? (
          todos.map(todo => {
            return (
              <li key={todo._id} onClick={() => deleteTodo(todo._id)}>
                {todo.action}
              </li>
            );
          })
        ) : (
          <li>No todo(s) left</li>
        )
      }
    </ul>
  );
}

export default ListTodo;
```
![Screenshot (173)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e333b241-b83d-479c-b917-8e4b9f943127)

 in the Todo.js file, write the following code
 ```
vim Todo.js
```
paste the code below
```
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;
    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}

export default Todo;
```
![Screenshot (174)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3814da1d-74c8-478e-ba33-7dd4502b7f73)
We need to make a little adjustment to our react code. Delete the logo and adjust our App.js to look like this
Move to src folder
```
cd ..
```
Ensure to be in the src folder and run:
```
vim App.js
```
paste the code below
```
import React from 'react';
import Todo from './components/Todo';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <Todo />
    </div>
  );
}

export default App;
```
![Screenshot (175)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f986c814-a027-4b66-8138-67acdbfd6ac6)
In the src directory, open the App.css
```
vim App.css
```
copy and paste the code below
```
.App {
  text-align: center;
  font-size: calc(10px + 2vmin);
  width: 60%;
  margin-left: auto;
  margin-right: auto;
}

input {
  height: 40px;
  width: 50%;
  border: none;
  border-bottom: 2px #101113 solid;
  background: none;
  font-size: 1.5rem;
  color: #787a80;
}

input:focus {
  outline: none;
}

button {
  width: 25%;
  height: 45px;
  border: none;
  margin-left: 10px;
  font-size: 25px;
  background: #101113;
  border-radius: 5px;
  color: #787a80;
  cursor: pointer;
}

button:focus {
  outline: none;
}

ul {
  list-style: none;
  text-align: left;
  padding: 15px;
  background: #171a1f;
  border-radius: 5px;
}

li {
  padding: 15px;
  font-size: 1.5rem;
  margin-bottom: 15px;
  background: #282c34;
  border-radius: 5px;
  overflow-wrap: break-word;
  cursor: pointer;
}

@media only screen and (min-width: 300px) {
  .App {
    width: 80%;
  }

  input {
    width: 100%;
  }

  button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
  }
}

@media only screen and (min-width: 640px) {
  .App {
    width: 60%;
  }

  input {
    width: 50%;
  }

  button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
  }
}
```
![Screenshot (176)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/40891fa9-cdf1-4431-9984-11e6296cd4a7)
In the src directory, open the index.css
```
vim index.css
```
copy and paste the code below
```
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
```
![Screenshot (177)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1eea02fc-a102-41b1-84d2-3bcb25cf9ca9)

**Go to the Todo directory**
```
cd ../..
```
**Run the code below**
```
npm run dev
```
![Screenshot (178)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b1e3772d-4b51-4370-80b1-8aec3bb9924c)

## now our Todo App is ready for use and fully functional with creating a task , deleting a task and viewing all the task.

![Screenshot (181)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/147cdf06-4007-4125-8b07-53a0221e83fb)

# Conclusion:
Building a Todo app with the MERN stack (MongoDB, Express, React, and Node.js) is a comprehensive project that covers many essential aspects of full-stack web development. Through this project, I have learned how to:

**Set Up and Configure the Development Environment:** By installing and configuring Node.js, MongoDB, and related dependencies, I laid the foundation for the MERN stack application.
**Design and Implement RESTful APIs:** Using Express.js, I created a backend that handles CRUD operations, allowing me to manage todos effectively.
**Connect to a MongoDB Database:** With Mongoose, I defined schemas and models, making database interactions straightforward and structured.
**Build a Frontend Interface with React:** React.js helped me build a dynamic and responsive user interface, enabling users to interact with the Todo app seamlessly.
**Integrate Frontend and Backend**By setting up proxy configurations and using asynchronous API calls, I connected the frontend with the backend, achieving a fully functional application.
**Implement State Management:** I managed state effectively within React components, ensuring data consistency and improving user experience.
**Handle Errors and Debugging:** Through logging and error handling, I ensured the robustness of the application, making it more reliable and easier to maintain.
**Testing API using Postman**

















