# WebStackImplementation-LEMP_Stack
## Cementing the Skills of Deploying Web Solutions using the LA(E)MP Stacks

### STEP 0: THE PREREQUISITES
- Catch up registering and setting up an AWS free-tier account with Ubuntu Server OS via EC2, [**HERE**](https://github.com/yemikareem/LampStackImplementation#step-0-the-prerequisites)

NB: In the previous project, we used Putty on Windows to connect to EC2 Instance; now we will use the simplest option, GitBash.

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

- Ref [Project LampStack](https://github.com/yemikareem/LampStackImplementation#step-1-installing-apache-and-updating-the-firewall): rule already added to EC2 configuration to open inbound connection via TCP port 80 

- Checking how to access it locally in our Ubuntu shell - "curl http://localhost:80" or "curl http://52.56.228.186:80" 

  NB: Another way to retrieve the Public IP Address instead of checking it in the AWS console is by typing this directly in the CLI - "curl -s http://169.254.169.254/latest/meta-data/public-ipv4"

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/3f6a6f0e-addd-4bea-a741-671524d7d036)

  Well, this is the same content gotten by the 'curl' command but represented in a nice HTML format by the web browser.


### STEP 2: INSTALLING MYSQL
Please refer to [**Step 2, Project 1**](https://github.com/yemikareem/LampStackImplementation#step-2-installing-mysql) for a detailed process on how to install MySql.


### STEP 3: INSTALLING PHP
Tip: *Linux** is installed to serve as a work environment, *Nginx** (instead of Apache) is installed to serve the content (this is the main difference in LAMP & LEMP stacks), *MySQL** is installed to store and manage data, and *PHP** processes code and generate dynamic content for the web server.

Apache embeds the PHP interpreter in each request, while Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. 

So, we'll need to install php -fpm , which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. 

Additionally, we'll need php-mysql , a PHP module that allows PHP to communicate with MySQL-based databases.

Core PHP packages will automatically be installed as dependencies.

- To install these 2 packages at once, run: "sudo apt install php-fpm php-mysql"

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/fbba2b02-26e1-4b41-8de9-2ad68cd66759)


### STEP 4: CONFIGURING NGINX TO USE PHP PROCESSOR 
Tip: In Nginx web server, we can create server blocks (like virtual hosts in Apache) to contain configuration details and host more than one domain on a single server. 

Using projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default which is configured to serve documents out of a directory at /var/www/html . This will work for a single site, but can become difficult to manage when hosting multiple sites. 

Don't modify /var/www/html , rather create a directory structure within /var/www for the our_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

- Create the root web directory for your_domain as follows: "sudo mkdir /var/www/projectLEMP" 
- Assign ownership of the directory with the $USER environment variable, referencing the current system user - "sudo chown -R $USER:$USER /var/www/projectLEMP"

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/fe1b7206-a437-4f65-9195-e4e74b5e128b)

- Open a new configuration file in Nginx's sites-available directory, using the preferred command-line editor - we will be using **nano* - "sudo nano /etc/nginx/sites-available/projectLEMP" - this creates a new blank file

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/f9309556-d6b0-4bdc-9d3e-5bf93dff3c69)

- Paste in the below bare-bones configuration - use Ctrl + Right click mouse + Paste

   #/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/c354017f-becc-478d-96ca-4b4324822aad)

#### WHAT EACH OF THE DIRECTIVES AND LOCATION BLOCKS DO:
- *listen* — Defines what port Nginx will listen on. In this case, it will listen on port *80* , the default port for HTTP.

- *root* — Defines the document root where the files served by this website are stored.

- *index* — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list

- *index.html* files with a higher precedence than *index.php* files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.

- *server name* — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server's domain name or public IP address.

- *location /* —Thefirstlocationblockincludesa *try files* directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

- *location ~ \.php$* — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the *php7.4-fpm.sock file* , which declares what socket is associated with *php-fpm* .

- *location ~ /\.ht* — The last location block dealswith *.htaccess* files, which Nginx does not process. By adding the deny all directive, if any *.htaccess* files happen to find their way into the document root they will not be served to visitors.


When done editing, save and close the file. because we are using nano, we can do so by typing Ctrl+X, then y and ENTER to confirm 

- Activating the configuration by linking to the config file from Nginx's sites-enabled directory, will tell Nginx to use the configuration the next time it is reloaded - "sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/"

- Testing the configuration for syntax error - "sudo nginx -t"

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/57fa69cf-96e1-427c-a931-f88f6f5483d9)

- There is a need to disable the default Nginx host that is currently configured to listen on port 80 - "sudo unlink /etc/nginx/sites-enabled/default"
- Then reload Nginx to apply the changes - "sudo systemctl reload nginx"

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/9f39ae72-1f90-445f-96b0-22524c8126ba)

At this point, our new site is active, but the web root /var/www/projectLEMP is still empty. 

- To create an index.html file in that location to test that the new server block is working perfectly - "sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html"

- Now, let's go to our browser to open our website URL using IP address

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/564f8193-f19f-4e85-8275-935abd6e7c0b)

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/27fcc486-c859-42d7-8248-2fc866385d1f)

NB: This can be left in place temporarily as a landing page until index.php is set up for replacement


### STEP 5: TESTING PHP WITH NGINX 
- To test that Nginx can handle .php file to the PHP processor, create a test PHP file in the document root. Open a new file called info.php within the document root in the text editor - "nano /var/www/projectLEMP/info.php"

- Type or paste the following:
"<?PHP"

"phpinfo();"

- When done, save and close the file in the text editor. Refresh the page by adding /info.php to the URL, and the below will load up:

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/5aa62f0d-bded-4f4d-86d3-26e1c4f7e350)

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/b08dd279-bbf0-4e8f-8064-c6ee1ffe2092)

Please note that it is best to remove the created file after checking the relevant information through this page, as it contains sensitive information about the PHP environment and the Ubuntu server.

- To remove files that contain sensitive information - "sudo rm /var/www/your_domain/info.php" 

This can always be regenerated whenever needed. 


### STEP 6: RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
**Objective:** To create a test database (DB) with a simple "To Do List", and configure access to it, so that the Nginx website would be able to query data from the DB and display it. 

NB: Because the native MySQL PHP library *mysqlnd* doesn’t support *caching_sha2_authentication* yet, which is the default authentication method for MySQL 8. We'll need to create a new user with the *MySQL_native_password* authentication method in order to be able to connect to the MySQL database from PHP.

We will create a database named *example_database* and a user named *example _user*. We can on our own replace these names with different values.

- First, let's connect to the MySQL console using the root account: "sudo MySQL".

NB: If you are not the root user, first enter on the CLI - "sudo mysql -p", then enter the root password

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/0de8872a-7add-4b51-8c0f-76b6231f53d3)

- To create a new database - "mysql> CREATE DATABASE `example_database`;"


