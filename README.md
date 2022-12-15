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












