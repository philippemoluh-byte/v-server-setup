# v-server-setup

This v-server is based on linux and can be used for development, testing, and hosting applications.

## Description 

This document provides detailed instructions for installing and configuring a nginx web server for the static Site hosting and cloning a repository on the V-server.

## Table of contents
- Prerequisites
- Quickstart
- Usage

### Prequisites
- Linux (Ubuntu 24.04.3 LTS)
- Git (Knowledge on Git Operation)
- Editor like vim or nano

### Quickstart 

1. Generate a ssh key pair on the local machine

```
ssh-keygen -t ed25519 -C "ihre_email@beispiel.de"

```
2. Connect to the server

```
ssh user@host

```
3. Copy the ssh public key to the server

```
ssh-copy-id -i ~/.ssh/your_key.pub user@host

```

### Usage 

- For the connection to the server using the ssh key or passwort ensure that you are disconnected from the server at first. 
- Ensure that the connection using ssh key is successful before to disable the passwort


1. #### Connect to the v-server using the ssh key

    ```
    ssh -i ~/.ssh/your_key  user@host

    ```

2. #### Setup the server configuration file to disable the login using the password

    - Update the ssh configuration file by changing the  "PasswordAuthentication"  to "no"

        ```
            sudo nano /etc/ssh/sshd_config
            PasswordAuthentication no

        ```
    - Save the ssh configuration file file and restart the SSH service to apply the change

        ```
        sudo systemctl restart sshd

        ```
    - Check if the password is succesfull disabled in the server
    by connecting with ssh key
        ```
        ssh -i <path/to/ssh-key -o PubkeyAuthentication=no user@host 

        ```
3. #### Install the nginx webserver for in the V-server

    - Update your local package to get the latest version

        ```
        sudo apt update

        ```
    - Install the Nginx webserver
        ```
        sudo apt install nginx -y

        ```
    - Test the Nginx configuration
        ```
        sudo nginx -t

        ```
    - Call the host on the browser and check if you see the Welcome page of Nginx webserver

4. #### Configure the NGINX server to display alternative HTML page.

    - Ensure that the directive exists
        ```
        ls /var/www

        ```
    - Create the directory "mywebsite"
        ```
        mkdir /var/www/mywebsite

        ```
    - Create a html file in "mywebsite" directory by running
        ```
        sudo touch /var/www/mywebsite/page-index.html

        ```
    - Edit the html file by adding a Html contain. after save the file
    
    - Add a configuration file to enable the display of the alternative html page

        ```
        ssudo nano /etc/nginx/sites-enabled/mywebsite

        ```
        ``` Json
        {
            listen port;
            listen [::]:port;

            root /var/www/mywebsite;
            index page-index.html;

            location / {
                try_files $uri $uri/ =404;
                }
        }

        ```
    - Save and leave the file
    - Retart the nginx server
        ```
        sudo service nginx restart

        ```
    - Call the the following link on your browser to see the new page
        ```
        host:port

        ```

5. #### Configure the git in the v-server 

    - To generate the ssh-key in the v-server see the Quickstart

    - Copy the contain of the public key of the v-server and save it in the Github repository

    - Configure the git on the server to use the same username and email as on GitHub.
        ```
        git config --global user.name "user name"
        git config --global user.email "ihre_email@beispiel.de"

        ```
    - Clone the git repository
        ```
        git clone https://github.com/philippemoluh-byte/v-server-setup.git
        
        ```
















