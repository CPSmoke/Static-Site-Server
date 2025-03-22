# Static-Site-Server
https://roadmap.sh/projects/static-site-server
1. Setting Up a Remote Server
Choosing a Provider: Sign up for DigitalOcean, AWS, or another provider and create a new Linux server instance (usually Ubuntu or CentOS).

Connecting via SSH: Use SSH to connect to your server. Open a terminal and run the following command:

    ssh username@your_server_ip
  
Replace username with the username (usually root) and your_server_ip with the IP address of your server.

2. Installing Nginx
Install Nginx: After connecting to the server, run the following commands to install Nginx:

        sudo apt update
        sudo apt install nginx
Starting Nginx: Ensure that Nginx is running:

    sudo systemctl start nginx
    sudo systemctl enable nginx
3. Creating a Static Website
Create the Website Structure: Create a directory for your website:

        sudo mkdir -p /var/www/your_website
Create a Simple HTML File: Create an index.html file in the website directory:


        sudo nano /var/www/your_website/index.html
Insert the following code:

        html
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>My Static Website</title>
            <link rel="stylesheet" href="style.css">
        </head>
        <body>
            <h1>Welcome to My Static Website!</h1>
            <p>This is a simple static website hosted on Nginx.</p>
        </body>
        </html>
Create a CSS File: Create a style.css file for styles:


    sudo vim /var/www/your_website/style.css
Insert styles as you wish, for example:

    css
    body {
        font-family: Arial, sans-serif;
        background-color: #f4f4f4;
        color: #333;
        text-align: center;
    }
4. Configuring Nginx
Create a Configuration for Your Site:


        sudo vim /etc/nginx/sites-available/your_website
Insert the following configuration:


    server {
        listen 80;
        server_name your_domain_or_ip;
    
        root /var/www/your_website;
        index index.html;
    
        location / {
            try_files $uri $uri/ =404;
        }
    }
Replace your_domain_or_ip with your domain or IP address.

Activate the Configuration:


    sudo ln -s /etc/nginx/sites-available/your_website /etc/nginx/sites-enabled/
Check Configuration and Restart Nginx:

    sudo nginx -t
    sudo systemctl restart nginx
5. Using rsync to Update the Site
Create a deploy.sh Script:


        vim deploy.sh
Insert the following code:


    #!/bin/bash
    
    rsync -avz --delete ./ /var/www/your_website/
Make the Script Executable:


    chmod +x deploy.sh

6.Errors
6.1 Checking Access Permissions:
Make sure that the user under which NGINX is running has access permissions to the directory /var/www/your_website and its contents. Typically, this user is www-data on Ubuntu. You can change the access permissions with the following commands:

    sudo chown -R www-data:www-data /var/www/your_website
    sudo chmod -R 755 /var/www/your_website
6.2 Checking for the index.html File:
Ensure that the file index.html actually exists in the directory /var/www/your_website. If it’s missing, the server will not be able to display the page.

6.3Checking NGINX Configuration:
Make sure the NGINX configuration is correct. You can check the configuration using the command:

    sudo nginx -t
If there are any errors, fix them and restart NGINX:


    sudo systemctl restart nginx
6.4 Checking SELinux (if enabled):
If you have SELinux enabled (this may be relevant if you are using other distributions, such as CentOS), make sure that SELinux is not blocking access to your files.

6.5 NGINX Logs:
Check the NGINX error logs for additional information about what is causing the 403 error. Logs are usually located at /var/log/nginx/error.log:

    cat /var/log/nginx/error.log


7. Updating Your Domain (if you have one)
If you have a domain name, point it to your server's IP address through your domain management panel (DNS).

