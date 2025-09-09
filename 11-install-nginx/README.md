# TRAINING - Install NGINX

NGINX is a powerful web server software used by several hosting companies. It was designed to handle a high volume of requests simultaneously. Therefore, it offers faster loading times and better performance than most other web servers.

Additionally, NGINX uses fewer resources and hardware than other server software. This characteristic makes it a cost-effective solution. It’s also compatible with a variety of web applications.

We will be setting up our NGINX web server and host a simple Hello World application 


## Install NGINX Web Server

Update the apt repositories
```bash
sudo apt update
```

install NGINX
```bash
sudo apt install nginx
```

Start nginx and set it to autorun on startup
```
# This starts NGINX now
systemctl start nginx
# This enables NGINX to start on boot
systemctl enable nginx
```

verify that our web server is up 

```bash
curl localhost
```

## Set up our own website

Create our application directory. We'll use the git repository from the last step
```bash
cd /var/www
git clone https://github.com/PureLogicIT/101-linux-certificates-git tutorial
cd tutorial/11-install-nginx
git checkout main
```
Verify that the index.html file is correct.
```bash
cat index.html
```

## Set up a virtual host on port 80

Create our application file
```bash
sudo cp tutorial.conf /etc/nginx/sites-available/
cd /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/tutorial.conf
```

Verify the content of *tutorial.conf*
```ini
server {
       listen 80;
       listen [::]:80;

       server_name tutorial.domsin.com;

       root /var/www/tutorial;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```

Restart our NGINX service
```bash
sudo service nginx restart
```

Test the application
```bash
curl localhost
```

 Now you can connect to the application on port 80
