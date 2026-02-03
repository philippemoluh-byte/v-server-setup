# v-server-setup

This guide describes how to set up a Linux-based virtual server for secure access and basic web hosting. It covers SSH key-based authentication, disabling password authentication, installing and configuring the NGINX web server for static HTML hosting, and cloning and configuring a Git repository directly on the server.

Key features

- Log in to the v-server using an SSH key
- Host an HTML page on the server
- Clone and configure a Git repository on the server

## Prerequisites

- Linux (Ubuntu 24.04.3 LTS)
- Git (knowledge of Git operations)
- An editor such as vim or nano

## Set up access to the server using an SSH key

To start configuring the web server, perform the following steps:

1. Generate an SSH key pair on the local machine

```bash
ssh-keygen -t ed25519 -C "<your_email_adress>"

```

1. Connect to the server

```bash
ssh user@host

```

1. Copy the ssh public key to the server

```bash
ssh-copy-id -i ~/.ssh/your_key.pub user@host

```

[!IMPORTANT]

- Before disabling password authentication, make sure your SSH key-based login works and that you are not locked out.

[!WARNING]

- Ensure the SSH key-based connection is successful before disabling password.

1. Connect to the v-server using the SSH key

```bash

ssh -i ~/.ssh/your_key  "<your_root_name>"@"<your_ip>"

```

## Disable password login

1. Update the SSH configuration file

```bash

    sudo nano /etc/ssh/sshd_config
    #set:
    PasswordAuthentication no
```

1. Save and close the SSH configuration file.

2. Restart the SSH service to apply the change

```bash
sudo systemctl restart sshd

```

1. Verify that password authentication has been disabled successfully.

```bash
ssh -i <path/to/ssh-key -o PubkeyAuthentication=no "<your_root_name>"@"<your_ip>"

```

## Install NGINX web server

1. Update package lists.

``` bash
sudo apt update

```

1. Install the NGINX package.

```bash
sudo apt install nginx -y

```

1. Test the NGINX configuration.

```bash
sudo nginx -t

```

1. Open the server address in a web browser.
2. Confirm the NGINX default welcome page is displayed.

## Configure the NGINX web server

1. Ensure that the directory exists.

```bash
ls /var/www

```

1. Create the directory "mywebsite"

```bash
sudo mkdir -p /var/www/mywebsite
```

1. Create an HTML file in the "mywebsite" directory by running.

```bash
sudo touch /var/www/mywebsite/page-index.html

```

1. Edit the HTML file and add simple HTML content.

```html
<!DOCTYPE html>
<html>
    <body>

        <h1>My First Heading</h1>
        <p>My first paragraph.</p>

    </body>
</html>
```

1. After saving and closing the file, add a site configuration to serve the HTML page.

```bash
sudo nano /etc/nginx/sites-enabled/mywebsite

```

Example site block(use a server block):

```nginx
{
    listen <your_nginx_port>; # Example 8070
    listen [::]:<your_nginx_port>;

    root /var/www/mywebsite;
    index page-index.html;

    location / {
        try_files $uri $uri/ =404;
        }
}

```

1. Save and close the file
2. Enable the site and restart NGINX:

```bash
sudo service nginx restart

```

1. Open the following address in your browser to see the new page:

```text
http://<your_ip>:<your_nginx_port>

```

## Configure and clone the Git repository

[!IMPORTANT]
Ensure that you are connected to the server.

1. Generate an SSH key on the server.

```bash
ssh-keygen -t ed25519 -C "<your_email_adress>"

```

1. Copy the contents of the public key and add it to your GitHub account.

2. Configure Git on the server to use the same username and email as your GitHub account.

```bash
git config --global user.name "<your_github_account_username>"
git config --global user.email "<your_github_account_email>"

```

1. Clone the git repository

```bash
git clone https://github.com/<your_github_account_name>/<your_github_repository_name>.git

```

### Ensure your SSH connection to GitHub works

1. Run the following command on your server.

```bash
git -T git@github.com

```

1. You should see a message asking to verify the host fingerprint. Verify it matches GitHub's public key and type yes to continue.

[!NOTE]
You may see this message on successful authentication:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.

```
