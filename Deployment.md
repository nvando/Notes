# Deploying Projects

**1. Setting up a Virtual Private Server:** 
A siloed space on a server that has the characteristics of an entire server itself, with its own Operating System (OS), applications, resources, and configurations.  

**2. Setting up the webserver NGINX**  
Accepts requests, takes care of general domain logic and handles https connections  
**3. Setting up the application server Gunicorn**  
**4. Running a flask application on the server**  
 



## 1. Setting up a VPS



### What is a virtual private server? 
A server is a computer dedicated to storing information or an application and making that info/app available over a network. A server can refer to multiple concepts:

- *Hardware server*: the actual physical computer.  
- *Server software*: software (such us NGINX) that tells the network where to find the stored information and/or runs your app. 
- *Virtual private server*: software that essentially pretends to be a full hardware computer, upon which server software is also run.

A basic server:
![A basic server](basic_server.png)
Multiple VPS on a harware server:
![vps](vps.png)

[READ: A beginners guide to to VPS hosting](https://www.websiteplanet.com/blog/what-is-vps-hosting/)

### Setting up a VPS on Digital Ocean
 1. Create an account with Digital Ocean, a cloud hosting provider. 
 2. Create a VPS, called Droplets on Digital Ocean
 3. Select an image to be copied to your server. An image is a clone of a pre-installed version of distribution, such as Ubuntu (Linux). This copy will be the inistial state of your server.  Ubunutu is the most well known and user friendly distribution for a linux system. linux is generally preferred over a window operating system, because most versions are free of charge. 


 4. Add an SSH key  
   
    *SSH keys are a pair of public and private keys that are used to authenticate and establish an encrypted communication channel between a client and a remote machine over the internet.*

    Your SSH jey is stored in the ~/.ssh/ folder. They can be viewed by 
    ```
    $ ls -al ~/.ssh
    ```  
    This will also show a known_hosts file. This file keeps track of all servers (hosts) which you have connected with before and you trust as a safe host. 

    The keys can be viewed by the commands:
    ```
    cat ~/.ssh/id_rsa
    ````

     ```
    cat ~/.ssh/id_rsa.pub
    ````

    [Read about SSH keys](https://dev.to/risafj/ssh-key-authentication-for-absolute-beginners-in-plain-english-2m3f)  
    [Read on generating a SSH key on Windows](https://www.howtogeek.com/762863/how-to-generate-ssh-keys-in-windows-10-and-windows-11/)

 5. Choose a datacenter near you, to prevent latency while working on the server. In practice you will pick this based on where your customers are located.
 6. Choose a name for your project and click create droplet to set up your VPS.

 7. You can log in to the server via the terminal. Use the IP address shown in Digital ocean and the command:  
    ```
    ssh root@<ip-address>
    ```

    Accept the authenticy of the host if asked.If accepted, a fingerprint will be added to the know_host file in the ~/.ssh folder.   
    Commands you enter while logged in are not executed on your own computer but are instead sent to the remote device and executed there. This is only the case for this specific session, other windows are not logged in to the server. If you close the window, you'll have to log back in again in a new session.

8. Update all Ubuntu packages installed on the VPS using the package manager 'apt' (Advanced Package Tool), as the initial (Ubuntu) image copied to the server may be outdated.  Use the command ```$ apt update``` to update the package sources list with the latest versions of the packages in the repositories. Then use ```$ apt upgrade``` to compare the version of all the packages currently installed on the system with the ones in the list we fetched through ```$ apt update``` and upgrade all of them to their latest versions.

 


