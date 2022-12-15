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


