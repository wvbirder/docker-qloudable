
# Docker Workshop

![](images/000JumpStart/TitleJS.png) 

Updated: June 23, 2018

## Oracle Cloud Infrastructure Overview

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

 ### **STEP 2**: AlphaOffice Application

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

## Explore your Docker environment

You will run some Docker commands to see details of the installation and the deployment of the AlphaOffice containers

### **STEP 1**: Run some Docker Commands

Open a terminal session and look into the set up

- On the desktop **right-click** and select **Open Terminal**

    ![](images/000JumpStart/JS16.png)

- **Type** the following:

```
su - opc
docker version
```

This takes us into the "opc" user and shows the current Docker version (`18.03`)

![](images/000JumpStart/JS17.png)

- **Type** the following to see what Docker containers are running:

```
docker ps
```

  ![](images/000JumpStart/JS18.png)

The output shows three containers running named:
```
alphaofficeui
restclient
twitterfeed
``` 
The unique container ID that docker assigns at runtime is shown along with the startup command and the networking ports that the containers have expoused. These are mapped to the HOST's to the same ports on the HOST for external cosumption. The TwitterFeed Java application is running on port 9080, the RESTClient is on port 8002 and the AlphaOffice UI is on port 8085.

Docker uses a default network called `bridge` and assigns virtual IP addresses to each container. Any containers on on the same networks can implicity see each other.

- **Type** the Following:

```
docker network ls
```

  ![](images/000JumpStart/JS19.png)

- `docker inspect` will show us all details of a particular container. Storage locations, storage volumes, storage types, networking subnet and IP address and much more. We will run the following to get information on the `restclient` container. **Type** the following:

```
docker inspect restclient
```

Scroll through the JSON output. We will touch on a couple of sections:

  ![](images/000JumpStart/JS20.png)

The output above shows the Creation Date, if the container is running, The process ID (`2351`) on the HOST operating system, the path location on the HOST where inforamtion is stored (`/var/lib/docker/...`), the type of storage Overlay that Docker is using on the HOST opearating system (`overlay2`)

  ![](images/000JumpStart/JS21.png)

The output above shows the arbitrarily assigned hostname (You can give the container a hostname on startup if you want), and the HOST exposed network ports (`8002`)

  ![](images/000JumpStart/JS22.png)  

The output above shows networking specifics for the container. The Docker virtual network that it's on (`bridge`), the ports the container is using (`8002`) and the assigned IP address (`172.17.0.4`)

 - **Cut and Paste** the following to get a list of all IP addresses of the current containers:

 ```
 docker ps -q | xargs -n 1 docker inspect --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}} {{ .Name }}' | sed 's/ \// /'
 ```
Example output (your IP assignments may vary):

  ![](images/000JumpStart/JS23.png)  

# Make changes to the AlphaOffice application

In this section you will make a couple of changes to the AlphaOfficeUI application. One will correct a typo and another will change the background image. The flow for this will be as follows:

- Copy the new background image into the container
- Connect into the AlphaOfficeUI container
- Install the vim text editor
- Fix a typo and specify the new background image
- Save (docker commit) a copy of the changes to a NEW docker image
- Start up and test the AlphaOfficeUI container using the NEW image

## Container In-place Modifications

### **STEP 1**: Copy a New Background Image

Copy a background image file into the running AlphaOfficeUI container. This file is in the <YOUR_HOME>/AlphaOfficeSetup directory that you GIT cloned at the beginning of the lab

**NOTE:** Make sure you are still logged in as the `opc` user

- **Type** the following:

```
docker cp /home/opc/AlphaOfficeSetup/dark_blue.jpg alphaofficeui:/pipeline/source/public/Images
```

  Example: docker cp /home/opc/AlphaOfficeSetup/dark_blue.jpg alphaofficeui:/pipeline/source/public/Images

### **STEP 2**: Install the VIM editor in the UI container

Even though the orginal AlphaOfficeUI image could have been set up ahead of time with any needed client tools we're adding the the environment on-the-fly to give you some idea that it can be done

- Connect into the `alphaofficeui` container:

```
docker exec -it alphaofficeui bash
```

- **Type** the following:

```
apt-get update
```

![](images/000JumpStart/Picture200-28.png)

- **Type** the following:

```
apt-get install vim
```
- **NOTE: If may see output that says it's already the newest version**

- If applicable, say **Y** at the "Do you want to continue?" prompt.

- Verify the "**dark_blue.jpg**" file is in the container by **typing**:

```
ls /pipeline/source/public/Images
```

![](images/000JumpStart/Picture200-28.1.png)

### **STEP 3**: Edit the alpha.html file   

- Edit the `alpha.html` file to fix a typo - Note, if you are unfamiliar with `vim`, you'll find information at this URL: [VIM](http://vimsheet.com). The commands are very similar to vi:

```
vim /pipeline/source/public/alpha.html
```

- Move the cursor to the text you wish to edit and press the letter __i__ to make changes. Fix the header title to read "**Alpha Office Product Catalog**". You can also change the body title to whatever you want:

![](images/000JumpStart/Picture200-29.png)

- Save the file and exit by hitting the **ESC** key and then holding the **SHIFT** key down and typing "**Z**" TWICE

### **STEP 4**: Edit the alpha.css file

- **Type** the following:

```
vim /pipeline/source/public/css/alpha.css
```

- Change the background image reference to "**dark_blue.jpg**"

![](images/000JumpStart/Picture200-30.png)

- Save the file and exit by hitting the **ESC** key and then holding the **SHIFT** key down and typing "**Z**" TWICE

- **Exit** out of the container:

```
exit
```

## Commit and run the NEW version of AlphaOffice

Commit and Test the changes

### **STEP 1**: Commit a NEW Docker image

In this step you will save a copy of your modifed docker container and give it a new name. You're back out in the HOST now. Substitute your docker hub account name and give the new image a name where asked for in the following commands:

- **Type** in following:

```
docker commit alphaofficeui (your-dockerhub-account)/(image-name)
```
  
- Example: `docker commit alphaofficeui wvbirder/alphaoffice-new`

- **Type** the following:

```
docker images
```

 - See the new saved image:

![](images/000JumpStart/Picture200-31.png)

### **STEP 2**: Start a container based on your new image

We will now stop and remove the old version of AlphaOfficeUI and fire off a new container based on your changes.

- **Type** the following:

```
docker stop alphaofficeui
docker rm alphaofficeui
```

- Start a new container using your new Docker image. **Cut and Paste OR Type**: (Substituting your DockerHub account name and the image name you created in Step 1)

```
docker run -d --name=alphaofficeui -p=8085:8085 (your-dockerhub-account)/(image-name)
```

- Example: `docker run -d --name=alphaofficeui -p=8085:8085  wvbirder/alphaoffice-new`

- Verify the new container is running by **typing**:

```
docker ps
```

![](images/000JumpStart/Picture200-32.png)

- Open up a new browser tab and **enter**:

```
http://<Public-IP>:8085
```

![](images/000Jumpstart/Picture200-33.png)
