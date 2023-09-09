# WebStackImplementation-LEMP_Stack
## Cementing the Skills of Deploying Web Solutions using the LA(E)MP Stacks

### STEP 0: THE PREREQUISITES
- Catch up registering and setting up an AWS free-tier account with Ubuntu Server OS via EC2 [HERE](https://github.com/yemikareem/LampStackImplementation#step-0-the-prerequisites)

NB: In the previous project, we used Putty on Windows to connect to EC2 Instance.

- To connect to EC2 Instance without converting .pem key to .ppk - using GitBash
  1. [Download and install](https://www.youtube.com/watch?v=qdwWe9COT9k) GitBash
  2. Launch GitBash and run - "ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>"
![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/d2d352f1-8dec-49ba-8ff4-974b105cdb85)

### STEP 1: INSTALLING THE NGINX WEB SERVER
Nginx is a high-performance web server that helps display web pages to site visitors.
- To install Nginx:
  1. Run "sudo apt update"
     ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/f73c1b57-6273-411b-bdb5-5015a05d1a2c)

  2. Then run "sudo apt install nginx"
     ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/7a3a623d-4858-434f-8997-91c7bc0da394)

- To test the Nginx successful file configuration - "sudo nginx -t"
  ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/b20b6e1c-d373-40e3-b1e5-4983b3d2aa02)

- To verify that nginx was successfully installed and is running as a service in Ubuntu - "sudo systemctl status nginx"

  NB: If it fails to load (see the image below), it may be that Apache is installed on the server. 
  ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/06e2168c-3fd1-4b61-a9c1-e1cbf3497d53)

  To check if Apache was installed on the server - "apachectl -v"

  ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/872d4be8-b646-4351-a542-faee02f806b2)

  Yes, it was. Now let's uninstall Apache - "sudo apt-get purge apache2", then "sudo apt-get autoremove apache2", then check again with "apachectl -v"
  ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/1ddbb431-d772-47aa-af65-87be2cac19cc)

  Restart Nginx - "sudo systemctl restart nginx"
  ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/0620a163-b326-4074-8b0b-c03eebbf542b)

  Now run "sudo systemctl status nginx" over again
![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/8b43bd75-6e1b-424f-901e-8caaafa0ba51)

- Ref [Project LampStack](https://github.com/yemikareem/LampStackImplementation#step-1-installing-apache-and-updating-the-firewall), rule already added to EC2 configuration to open inbound connection via TCP port 80 

- Checking how to access it locally in our Ubuntu shell - "curl http://localhost:80" or "curl http://52.56.228.186:80" 

  NB: Another way to retrieve the Public IP Address instead of checking it in the AWS console is by typing this directly in the CLI - "curl -s http://169.254.169.254/latest/meta-data/public-ipv4"
  ![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/3f6a6f0e-addd-4bea-a741-671524d7d036)

  Well, this is the same content gotten by the 'curl' command but represented in a nice HTML format by the web browser.


### STEP 2: INSTALLING MYSQL




### STEP 3: INSTALLING PHP
