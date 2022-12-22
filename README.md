# SIMPLE TO-DO APPLICATION ON MERN WEB STACK
*In this project you're tasked with implementing a web solution based on MERN stack in AWS cloud.*  

**MERN** Web stack consists of: 
- **MongoDB:** A document-based, No-SQL database used to store application data in a form of documents.  

- **ExpressJS:** A server-side Web Application framework for Node.js.  

- **ReactJS:**  A frontend framework developed by Facebook. It is based on JavaScript, and used to build User Interface (UI) components.  

- **Node.js:** A JavaScript runtime environment, used to run JavaScript on a machine rather than in a browser.  

![Screenshot_20221215_130237](https://user-images.githubusercontent.com/105195327/207854569-28033a7e-6b42-471f-9ceb-9fb2b3495cb9.png)  
  
  
As shown in the illustration above, a user interacts with the ReactJS UI components at the application front-end residing in the browser. This frontend is served by the application backend residing in a server, through ExpressJS running on top of NodeJS.
Any interaction that causes a data change request is sent to the NodeJS based Express server, which grabs data from the MongoDB database if required, and returns the data to the frontend of the application, which is then presented to the user.
 

## Step0 - Preparing prerequisites 
In order to complete this project, you will need an AWS account and a virtual server with Ubuntu server OS. If you do not have an AWS account, create one [here](https://aws.amazon.com/resources/create-account/) and refer to [project2](https://github.com/StrangeJay/DevOps-Project2/edit/main/README.md) on instructions on how to set up your EC2 instance.  

**Note:** You can have multiple EC2 instances running at a time, but make sure to stop the ones you don't need at the moment, and terminate the ones you have no further use of. *Your public IP changes when you stop and restart an instance.*  

In previous tasks, we've been making use of putty to connect to our ec2 instances. For this project, we are going to explore the usage of windows terminal. Watch this videos to learn how to set up windows terminal on your PC.  
- [windows installation part1](https://youtu.be/R-qcpehB5HY)
- [windows installation part2](https://youtu.be/jsNIlK5s6pI)

---
---

### **TASK**
Deploy a simple **To-Do** application that creates To-Do lists like this: 

---
## step1 - BACKEND CONFIGURATION 
- Update Ubuntu 
> `sudo apt update`  

- Upgrade Ubuntu 
> `sudo apt upgrade`  
  
- Get the location of Node.js software from [Ubuntu Repositories.](https://github.com/nodesource/distributions#deb)  
> `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`  
  
install the Node.js on the server, with the following command:  
>`sudo apt-get install -y nodejs`  
  
**Note:** The command above, installs both `nodejs` and `npm`. NPM is a package manager for Node, like apt for Ubuntu. It's used to install Node modules & packages, and to manage dependency conflicts.  
![Screenshot_20221215_150020](https://user-images.githubusercontent.com/105195327/207881007-5c570c72-a452-416b-a9e4-7fae83f0959e.png)  
  
- Verify the node installation with the command below  
> `node -v`  

- Verify the NPM installation with the command below  
> `npm -v`  


### Application Code Setup 
- Create a new directory for your To-Do project:  
`mkdir Todo` 

- Confirm the creation of the Todo directory by running the `ls` command.  
- Change to the newly created directory by running the command `cd Todo`.  
 ![Screenshot_20221215_151322](https://user-images.githubusercontent.com/105195327/207883178-050f47ae-592a-4193-8b37-d6c0ce366905.png)  
   
 - Initialise your project by running the `npm init` command. This will create a new file named **package.json**. The file will contain information about your application and the dependencies that it needs to run. *Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.*  
 
![Screenshot_20221215_152045](https://user-images.githubusercontent.com/105195327/207884845-70a177b8-0e9c-40ce-8e0e-bcfe9a16bde9.png)  

- Run the command `ls` to confirm that you have package.json file created.  
- Install ExpressJs and create the Routes directory.  

---
### INSTALL EXPRESSJS  
Express is a framework for Node.js, it takes care of a lot of things developers would have programmed straight out the box. *it simplifies development, and abstracts a lot of low-level details. For example, it helps to define routes of your application based on HTTP methods and URLs*  

- Install express using npm:  
> `npm install express`  

- Create a file `index.js` with the command below: 
> `touch index.js`  

- Run `ls` to confirm that your index.js file has been successfully created. 
 
- Install the `dotenv` module 
> `npm install dotenv`  

- Open the index file by running the following command: 
> `vim index.js`  

- Type the code below into the vim editor, and save it. *Do not get overwhelmed by the code you see. For now, simply paste the code into the file.  
![Screenshot_20221215_172734](https://user-images.githubusercontent.com/105195327/207915029-3c10cc07-35da-4606-9a0c-4403d00d227d.png)  
  
*Notice that we have specified the use of port 5000 in the code. This will be required later when we go on the browser.*  
Use `:wq` to save and exit vim. 

Now, it's time to start our server and see if it works.  
- Open your terminal in the same directory as your index.js file and type: `node index.js`  
*If everything goes well, you should see **`Server running on port 5000`** in your terminal. 
![Screenshot_20221215_173248](https://user-images.githubusercontent.com/105195327/207916194-983b83f2-f433-4d75-9b7d-fe38f692c80d.png)  
  
- Open port 5000 in EC2 Security Groups. By editing security group, selecting Custom TCP and typing in 5000 for port range  
![custom TP](https://user-images.githubusercontent.com/105195327/207917899-1a980d58-aaae-45bb-81a2-7a0f82fe9a23.png)  

- Open your browser and try to access your server's Public IP followed by port 5000: 
> `18.220.90.180:5000`   
  
**Note:** You can find your public IP in your AWS web console EC2 details, or run  
  
> `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` 

![Screenshot_20221215_174358](https://user-images.githubusercontent.com/105195327/207918553-83279350-279f-4900-bf4f-bc29408ec1d1.png)  
  

### Routes  
Our To-Do application needs to be able to carry out three actions: 
- Create a new task 
- Display a list of all tasks 
- Delete a completed task 

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: **POST, GET, DELETE.**
We need to create routes for each task, that will define various endpoints the To-do app will depend on. So let us create a folder **routes** 

- Create a folder by running the command below  
> `mkdir routes`  

- Change directory to the routes folder  
> `cd routes`  

- Create a file `api.js` with the command below  
> `touch api.js`  

- Open the file with the command below  
> `vim api.js`  

- Copy the code below in the file  
![Screenshot_20221215_180544](https://user-images.githubusercontent.com/105195327/207923217-d126eff0-9f1e-406a-a710-f046593398bd.png)  

Next, we will create the `Models` directory.  

---
### **MODELS** 
Since the app is going to make use of **Mongodb** which is a **NoSQL** database, we need to create a model. *A model is at the heart of JavaScript based applications, and it is what makes it interactive.*  
We will also make use of models in defining the database schema. *This is important to enable us define the fields stored in each Mongodb document.*  

- Install **mongoose** to create a Schema and a model. *mongoose is a Node.js package that makes working with mongodb easier.*  

- Change directory back to the Todo folder with cd .. and install Mongoose  
> `npm install mongoose`  
  
- Create a new folder **models**:  
> `mkdir models`  

- Change directory into the newly created ‘models’ folder with `cd models`  
  
- Inside the models folder, create a todo.js file  
> `touch todo.js`  
 
**Tip:** All three commands above can be defined in one line to be executed consequently with help of && operator, like this:
> `mkdir models && cd models && touch todo.js`  
  
- Open the file created with `vim todo.js` then paste the code below in the file:  
![Screenshot_20221215_193053](https://user-images.githubusercontent.com/105195327/207939698-a66b23ec-74db-4aa3-a1b0-ab13b3870d74.png)  
  
Now we need to update our routes from the file `api.js` in ‘routes’ directory to make use of the new model. 

- In Routes directory, open 'api.js' with `vim api.js`, delete the code inside with `:%d` command and paste the code below into it, then save and exit.  
![Screenshot_20221215_193542](https://user-images.githubusercontent.com/105195327/207940552-f35af13c-16df-49d9-adfd-a969a0f60ee2.png)  
  
---
### **MONGODB DATABASE**  
We'll be making use of mLAB for our databse needs, *mLab provides MongoDB database as a service solution (DBaaS).* To make life easy, you will need to sign up for a shared clusters free account [here.](https://www.mongodb.com/atlas-signup-from-mlab) 

- Follow the signup process 
![Screenshot_20221215_231905](https://user-images.githubusercontent.com/105195327/207980131-43c60d45-9af0-4c30-9ae3-ba32ceb00b2a.png)  

- You'll be sent an email for verification, click the click in the email and verify your account  
- After verification, resources to get started would be sent to your email 
- Click on shared cluster 

![Screenshot_20221215_232208](https://user-images.githubusercontent.com/105195327/207980220-4003f34c-b7b0-453e-b3d3-21f31ba9879c.png)  

- Select **AWS** as the cloud provider, and choose a region near you.  

**Complete a get started checklist**  
- [X] Build your first cluster  
- [X] Create your first database user  
- [X] Whitelist your IP address  
- [X] Load sample Data(optional)  
- [X] Connect to your cluster  

- Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)  
![Screenshot_20221216_000312](https://user-images.githubusercontent.com/105195327/207985841-417406b2-9f64-4f5d-87a4-383f5068cc39.png)  


**Important note**  
In the image below, make sure you change the time of deleting the entry from 6hours to 1 week. 
![Screenshot_20221216_000138](https://user-images.githubusercontent.com/105195327/207986121-0f37245e-f5d0-4cb0-aced-5bfb60e0d83a.png)

- Create a MongoDB database and collection inside mLab   
![DB Collections](https://user-images.githubusercontent.com/105195327/207987387-d94f3b97-9862-4113-8e59-b6697ea4cf90.png)  

![Screenshot_20221216_001722](https://user-images.githubusercontent.com/105195327/207987441-5b1a25dc-28ba-4b16-b5d1-891e1fe1d711.png)  


In the `index.js` file, we specified `process.env` to access environment variables, but we haven't created this file yet. So we need to do that now.  
- Create a file in your Todo directory and name it .env  
> touch .env  
> vi .env  

- Add the connection string to the access database in it, just as below:  
> `DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`  

*Ensure to update **username**, **password**, **network-address** and **dbname** according to your setup*  
  
- Get your connection string  
![cluster0](https://user-images.githubusercontent.com/105195327/207990895-17281924-2689-474a-89ac-579362094e21.png)  

![cluster connect](https://user-images.githubusercontent.com/105195327/207990911-966d2729-2e7b-4d9f-a764-360e6bdf79ac.png)  

![cluster js con](https://user-images.githubusercontent.com/105195327/207990920-dfd87a6c-904e-42ad-9c36-c26af9ca903d.png)  


Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database. *Simply delete the existing content in the file, and update it with the code below.* 

To do that using vim, follow the steps below:  
- Open the file with vim index.js
- Type :
- Type %d
- Hit ‘Enter’  
*The entire content will be deleted.* 
-Press `i` to enter *insert* mode in vim  
- Paste the code below  
  ![Screenshot_20221216_010335](https://user-images.githubusercontent.com/105195327/207992387-a39ce950-7dc4-4cd8-a96c-6a037ff32cc3.png)  
  
- Press esc 
- Type `:wq`   
  
**Note:** Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.  

- Start your server using the command: `node index.js`  
  
You should see a message ‘Database connected successfully’, if so – we have our backend configured. Now we are going to test it.  
  ![Screenshot_20221216_011036](https://user-images.githubusercontent.com/105195327/207992890-e9de984a-1a40-48b7-8faf-cf3b5794e35a.png)  

---
### Testing Backend Code Without Frontend Using RESTful API  
In this section, we will be using **Postman** to test our API.  
- Click [Install Postman](https://www.getpostman.com/downloads/) to download and install postman on your machine.  
- Click [Here](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform CRUD operartions on Postman  
  
Test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.
  
- Open Postman, create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. *This request sends a new task to our To-Do list so the application could store it in the database.*  
**Note:** make sure your set header key Content-Type as application/json  
![content type](https://user-images.githubusercontent.com/105195327/207997297-873ef898-3a5e-4f80-8826-e5eb98b49d75.png)  

- Create a POST request  
  ![post request](https://user-images.githubusercontent.com/105195327/207997480-b3bb26b6-9192-4c4b-942c-eec7304738bd.png)  
![Screenshot_20221216_014229](https://user-images.githubusercontent.com/105195327/207997596-c9b436be-e3fe-4cce-8ee9-860aac666116.png)  

- Create a GET request  
![get request](https://user-images.githubusercontent.com/105195327/207997657-78ce2cb7-103a-457d-9bd4-6f841b519808.png) 
*This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request)* 
  
- Create a DELETE request 
![Screenshot_20221216_015141](https://user-images.githubusercontent.com/105195327/207997774-6fe9f191-5162-4e65-8f71-65fe21c54f4c.png) 
*To delete a task – you need to send its ID as a part of DELETE request.*  
 ![Screenshot_20221216_015126](https://user-images.githubusercontent.com/105195327/207997810-de8f129c-9554-4c73-8e1b-52e8a89d3cbb.png)  
  

By now you have tested the backend part of your To-Do application and have made sure that it supports all three operations we wanted:  
- [X] Display a list of tasks – HTTP GET request  
- [X] Add a new task to the list – HTTP POST request  
- [X] Delete an existing task from the list – HTTP DELETE request  
  
We have successfully created our Backend, now let go create the Frontend... 
  
---
---

  ## **Step2 - FRONTEND CREATION** 
We're going to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of our To-Do app, we will use the `create-react-app` command to scaffold our app. 
  
- Run this code in the Todo directory:  
> `npx create-react-app client`  

This will create a new folder called **client** in your Todo directory, where you will add all the react code.  

### Running a React App  
Before testing the react app, there are some dependencies that need to be installed.

1. Install [concurrently](https://www.npmjs.com/package/concurrently). *It is used to run more than one command simultaneously from the same terminal window*  
> `npm install concurrently --save-dev`  
  
2. Install [nodemon](https://www.npmjs.com/package/nodemon). *It is used to run and monitor the server.* If there is any change in the server code, nodemon will restart it automatically and load the new changes.  
> `npm install nodemon --save-dev`  
  
3. In Todo folder, open the package.json file. Change the highlighted part of the below screenshot and replace with the code below  
> "scripts": {  
> "start": "node index.js",  
> "start-watch": "nodemon index.js",  
> "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""  
> },  
  
![Screenshot_20221216_123145](https://user-images.githubusercontent.com/105195327/208089784-5627ead8-c3ff-499b-9f49-203e612c6e52.png)  

![Screenshot_20221216_123523](https://user-images.githubusercontent.com/105195327/208089796-7af3f46a-b87e-4762-a053-4be7143f9271.png)  

### Configure Proxy in **package.json**   
1. Change directory to 'client'  
> `cd client`  

2. Open the package.json file    
> `vi package.json    

3. Add the key value pair in the package.json file "proxy": "http://localhost:3000"  
  
The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos.  
  
Ensure you are inside the Todo directory, and simply run:  
> `npm run dev`  
  
![Screenshot_20221216_131016](https://user-images.githubusercontent.com/105195327/208095524-0755eaa7-6d8c-4918-a753-e2ba0d9fa642.png)   

Your app should open and start running on localhost:3000  
![Screenshot_20221216_130853](https://user-images.githubusercontent.com/105195327/208095640-8801d207-a34f-4a13-b231-0e818dd49b43.png)  

  
**Important note:** To access the application from the Internet, you have to open TCP port 3000 on EC2 by adding a new Security Group rule. *Like we did previously, for port 5000.*  

### Creating Your React Components  
For our Todo app, there will be two stateful components and one stateless component. *One of the advantages of react is that it makes use of components, which are reusable and also makes code modular.*  

- Run the following cide from your Todo directory:  
> `cd client`  
  
- Move to the src directory  
> `cd src`  
  
- Inside your src folder create another folder called components  
> `mkdir components`  
  
- Move into the components directory with  
> `cd components`  
  
- Inside the ‘components’ directory, create three files **Input.js**, **ListTodo.js** and **Todo.js**.  
> `touch Input.js ListTodo.js Todo.js`  
  
- Open Input.js file  
> `vi Input.js`  
  
- Copy and paste the following  

> import React, { Component } from 'react';  
> import axios from 'axios';  
>  
> class Input extends Component {  
>  
> state = {  
> action: ""  
> }  
>  
> addTodo = () => {  
> const task = {action: this.state.action}  
>  
>     if(task.action && task.action.length > 0){  
>       axios.post('/api/todos', task)  
>         .then(res => {  
>           if(res.data){  
>             this.props.getTodos();  
>             this.setState({action: ""})  
>           }  
>         })  
>         .catch(err => console.log(err))  
>     }else {  
>       console.log('input field required')  
>     }  
>  
> }  
>  
> handleChange = (e) => {  
> this.setState({  
> action: e.target.value  
> })  
> }  
>  
> render() {  
> let { action } = this.state;  
> return (  
> <div>  
> <input type="text" onChange={this.handleChange} value={action} />  
> <button onClick={this.addTodo}>add todo</button>  
> </div>  
> )  
> }  
> }  
>  
> export default Input  
 
![Screenshot_20221216_131016](https://user-images.githubusercontent.com/105195327/208097681-2a4f4ffa-aa1f-43e2-bdd5-9ba7de420f1a.png)  

  
To make use of [**Axios**](https://github.com/axios/axios), *which is a Promise based HTTP client for the browser and node.js*, you need to cd into your Todo folder from your terminal and run `npm install axios`.  
  
- Move to the src folder
> `cd ..`  
  
- Move to clients folder  
> `cd ..`  
  
- Move to Todo folder  
> `cd ..`  
  
- Install Axios
> `npm install axios`  
  
- Go to ‘components’ directory  
> `cd client/src/components`  
  
- After that, open your ListTodo.js  
> `vi ListTodo.js`  
  
- In the ListTodo.js, copy and paste the following code  
![Screenshot_20221216_140010](https://user-images.githubusercontent.com/105195327/208103936-9eb21a4f-988b-4378-ab9b-f7d8fbad18ab.png)  

- In your Todo.js file, write the following code   
![Screenshot_20221216_140739](https://user-images.githubusercontent.com/105195327/208105206-2c784f58-5d84-4323-b0e0-b5a2e9c005bc.png)  
  
  
We need to make a little adjustment to our react code. Delete the logo and adjust our App.js.

- Move to the src folder  
> `cd ..`  
  
- Make sure that you are in the src folder and run
> `vi App.js`  
  
- Copy and paste the code below into it    
![Screenshot_20221216_141724](https://user-images.githubusercontent.com/105195327/208106784-f981c69f-e2c8-4e45-b764-8dd638b7c5fe.png)  

  
- After pasting, exit the editor.  
  
- In the src directory open the App.css  
> `vi App.css`  
  
- Paste the following code into App.css:  
> .App {  
> text-align: center;  
> font-size: calc(10px + 2vmin);  
> width: 60%;  
> margin-left: auto;  
> margin-right: auto;  
> }  
>  
> input {  
> height: 40px;  
> width: 50%;  
> border: none;  
> border-bottom: 2px #101113 solid;  
> background: none;  
> font-size: 1.5rem;  
> color: #787a80;  
> }  
>  
> input:focus {  
> outline: none;  
> }  
>  
> button {  
> width: 25%;  
> height: 45px;  
> border: none;  
> margin-left: 10px;  
> font-size: 25px;  
> background: #101113;  
> border-radius: 5px;  
> color: #787a80;  
> cursor: pointer;  
> }  
>  
> button:focus {  
> outline: none;  
> }  
>  
> ul {  
> list-style: none;  
> text-align: left;  
> padding: 15px;  
> background: #171a1f;  
> border-radius: 5px;  
> }  
>  
> li {  
> padding: 15px;  
> font-size: 1.5rem;  
> margin-bottom: 15px;  
> background: #282c34;  
> border-radius: 5px;  
> overflow-wrap: break-word;  
> cursor: pointer;  
> }  
>  
> @media only screen and (min-width: 300px) {  
> .App {  
> width: 80%;  
> }  
>  
> input {  
> width: 100%  
> }  
>  
> button {  
> width: 100%;  
> margin-top: 15px;  
> margin-left: 0;  
> }  
> }  
>  
> @media only screen and (min-width: 640px) {  
> .App {  
> width: 60%;  
> }  
>  
> input {  
> width: 50%;  
> }  
>  
> button {  
> width: 30%;  
> margin-left: 10px;  
> margin-top: 0;  
> }  
> }  

  
![Screenshot_20221216_143059](https://user-images.githubusercontent.com/105195327/208109722-2b04e974-f9fb-4b58-8784-87925d66f95b.png)  

  
### Exit
- In the src directory open the index.css  
> `vim index.css`  
  
- Copy and paste the code below:  
  
![Screenshot_20221216_143638](https://user-images.githubusercontent.com/105195327/208110066-a9dc2fbe-4f17-4dfe-a82b-85b6ec4c598b.png)  
  
- Go to the Todo directory  
> `cd ../..`  
  
In the Todo directory, run:  
> `npm run dev`  
  
In the absence of errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.  
![Screenshot_20221216_150144](https://user-images.githubusercontent.com/105195327/208114850-7f962a8b-e45c-4d19-bac5-4c8ca913e5d0.png)  

---
---  
#Congratulations  
In this Project #3 you have made a simple To-Do and deployed it to MERN stack. You wrote a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a MongoDB backend for storing tasks in a database.
  
