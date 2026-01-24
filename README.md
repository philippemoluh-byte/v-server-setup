# v-server-setup

## Add SSH-Key to the Vserver.
*Make sure that git bash or command promt or linux is installed in your local machine.<br />*
*Open the terminal and generate the ssh-key by using the folowing rules: **ssh-keygen -t ed25519 -C "ihre_email@beispiel.de"***

*Use the command **ssh user@188.245.120.189** and the password to connect to the server*

*After successful connection to the server use the following command to copy the ssh-key to the server:<br/> _ssh-copy-id -i ~/.ssh/your_key.pub user@188.245.120.189_*

**To check if the ssh key has been added correctly to the server** <br/> 
 * Ensure that you are not connected to the server. 
 * Give the following commd: **ssh -i ~/.ssh/your_key  user@188.245.120.189** *

## Disable the password in the Vserver.
1. Open SSH Config: sudo nano /etc/ssh/sshd_config (or vi)<br/><br/>
2. Modify Lines: Find and change these lines (remove '#' if present):
     * set PasswordAuthentication no
3. Save & Restart: Save the file and restart the SSH service: 
     * sudo systemctl restart sshd or 
     * sudo service ssh restart.

**To check if the password is succesfull disabled in the server** <br/> 
* Ensure that you are not connected to the server 
* run this command to connect with the username an password:<br/> 
    * ssh -i <path/to/ssh-private-key> \
            -o PubkeyAuthentication=no \
            -i pfad/zum/private-key \
            user@188.245.120.189
* Make sure that the connection fail

## Install the Webserver NGINX in the Vserver.
1. Ensure that you are connected to the server if not run the command : ssh -i ~/.ssh/your_key user@188.245.120.189 
2. Before installing new software, update your local package to get the latest version <br/>
    * **run :** *sudo apt update*
    * **Install NGINX :** *sudo apt install nginx -y*

**To check if the Installation of NGINX is successful** <br/> 
* Call this ip adress on your browser: **http://188.245.210.189/** 
* Make sure that you see the **Welcome to nginx! page** on the Browser 

## Configure the NGINX server to display alternative HTML page.
1. create page-index.html in the var/www/mywebsite directory.
      * Make sure that the directive exists by running: **ls /var/www**.
      * Create the directory **mywebsite**  by running: **mkdir /var/www/mywebsite**. 
      * Create a html file by running: **sudo touch /var/www/mywebsite/page-index.html*
      * Edit the html file by adding a Html contain. after save the file.
2. Add a configuration to enable the display of the alternative html page. under /etc/nginx/sites-enabled/

      * run sudo nano /etc/nginx/sites-enabled/mywebsite
      * add a the following contains: <br/>
      * server { <br/>
        1. listen 8081;
        listen [::]:8081;

        2. root /var/www/mywebsite;
        index page-index.html;

        3. location / {
            try_files $uri $uri/ =404;} <br/> 
            > } <br/>
    * save and leave the file,
    * retart the nginx server: **sudo service nginx restart**  
    * **To check if configuration of the alternative HTML page is successful:** <br/> 
        1. call this link on the browser: **188.245.120.189:8081**
        2. Make sure that you see a new Page on the browser

## configure the git in the v-server
* Ensure that you are connected to the server if not run the command : ssh -i ~/.ssh/your_key user@188.245.120.189 
* generate the ssh-key by using the folowing rules: **ssh-keygen -t ed25519 -C "ihre_email@beispiel.de"**
* after setup the path to install your key-pair click enter and make sure that the key-pair is generated.
* Go to the path containing the key pair and copy the contain of the public key
* Go to the Github and navigate to the **settings/keys** and click on the **new kew** to save the v-server ssh public key
* Go to the server and clone the v-server-setup repository: by giving: **git clone https://github.com/philippemoluh-byte/v-server-setup.git*

* go to the **v-server-setup** directory and do the git init and git pull the get the contain of the repository.





