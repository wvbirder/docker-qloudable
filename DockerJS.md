
# Docker Workshop

![](images/000JumpStart/TitleJS.png) 

Updated: June 23, 2018

## Oracle Cloud Infrastructure

With a comprehensive offering that includes compute, storage, network, bare metal, and container services, Oracle’s infrastructure services drive business value. Our enterprise-grade cloud enables organizations to run any workload at any time, while innovative migration tools pave the way by simplifying how organizations migrate on-premises workloads to the cloud.

This introduction provisions a Docker multi-container application running in the Compute Cloud Service on a VM Standard footprint. There are many options to choose from in Oracle's IaaS arena.

![](images/000JumpStart/JS0-2.png)

## Docker Overview

What is Docker? What is a container?

- Docker is the company and containerization technology.
- [Docker Documentation](https://docs.docker.com)
- A container is a runtime instance of a docker image: [Container Documentation](https://docs.docker.com/glossary/?term=container)

Containers have been around for many years. Docker created a technology that was usable by mere humans, and was much easier to understand than before. Thus, has enjoyed a tremendous amount of support for creating a technology for packaging applications to be portable and lightweight.

### VM vs Container

![](images/000JumpStart/JS1.png)

While containers may sound like a virtual machine (VM), the two are distinct technologies. With VMs each virtual machine includes the application, the necessary binaries and libraries and the entire guest operating system.

Whereas, Containers include the application, all of its dependencies, but share the kernel with other containers and are not tied to any specific infrastructure, other than having the Docker engine installed on it’s host – allowing containers to run on almost any computer, infrastructure and cloud.

## Lab Introduction

In this lab you will be looking at the various application components deployed in three Docker containers. The AlphaOffice application offers a list of products from a catalog. Refering to the `Product Catalog Application` section of the diagram below we see four Docker containers. 

**NOTE:** In your example deployment the "database" products are bundled in as part of the JS REST service via JSON. The full blown deployment, available as follow-up in-depth Labs, do use an Oracle or MYSQL database as its datasource.

The AlphaOffice UI container retrieves catalog information from a REST service (written in Node.js) running in a separate container. It also takes in sample Twitter feed data from a REST service (written in Java), also running in its own container, and combines the data into a unified front end.

![](images/000JumpStart/JS2.png)
![](images/000JumpStart/JS0-4.png)

## Objectives

- Connect into your account using VNC Viewer
- Do some docker commands to see various aspects of the set up
- Use the AlphaOffice application
- Go into the UI Docker container and make small changes in the application.

## Required Artifacts

- Once the infrastructure is provisioned you can access your enironment using `VNC Viewer`. Please download and install from: [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)

# Start the Demo Lab

Inside the Jump Start interface you will `Launch the Demo Lab`

![](images/000JumpStart/JS3.png)

- **In 6 minutes the Oracle IaaS infrastructure including the Application already running and deployed will be available.**

  ![](images/000JumpStart/JS4.png)

- While you're waiting, check out the short video that gives an overview of this session if you have already done so:

  ![](images/000JumpStart/JS5.png)

- When the environment is ready you will see the following along with the connect string to put into VNC Viewer. (In this example 129.213.56.126:1. Your IP address will be different)

  ![](images/000JumpStart/JS6.png)

- **You have 30 minutes** before the environment will go away.

  ![](images/000JumpStart/JS7.png)

## Log into your Account

Using VNC Viewer connect to the newly provisioned account.

### **STEP 1**: Start VNC Viewer

- Enter the connect string you were given (Example Shown):

  ![](images/000JumpStart/JS8.png)

- If presented with this prompt, just click **Continue**

  ![](images/000JumpStart/JS9.png)

- Enter the VNC password  **Qloudable** 

  ![](images/000JumpStart/JS10.png)

- You should now see your Desktop:

  ![](images/000JumpStart/JS11.png)

 ### **STEP 2**: Start VNC Viewer

 Check the AlphaOffice Application

- Click on the **Applications** tab and select **Firefox Web Browser**

  ![](images/000JumpStart/JS12.png)

- **Type** the URL **localhost:8085**

  ![](images/000JumpStart/JS13.png)

- The Application is displayed. **NOTE:** There is a typo in the tab. You will fix this in a few minutes...

  ![](images/000JumpStart/JS14.png)

- Click on the **Crayola Markers** product to see associated Twitter feeds.

  ![](images/000JumpStart/JS15.png)

- Minimize the browser. We will come back to that later.





