# Install NGINX

NGINX is a powerful web server software used by several hosting companies. It was designed to handle a high volume of requests simultaneously. It offers faster loading times and better performance than most other web servers.

Additionally, NGINX uses fewer resources and hardware than other server software. This characteristic makes it a cost-effective solution. It’s also compatible with a variety of web applications.

We will be setting up our NGINX web server and host a simple Hello World application.

## Install NGINX Web Server

Update the apt repositories

```bash
sudo apt update
```

Install NGINX

```bash
sudo apt install nginx
```

Start nginx and set it to autorun on startup

```bash
# This starts NGINX now
sudo systemctl start nginx
# This enables NGINX to start on boot
sudo systemctl enable nginx
```

Verify that our web server is up.

```bash
curl localhost
```

## Set up our own website

Create our application directory. We'll use the git repository from the last step.

```bash
cd /var/www
sudo git clone https://github.com/PureLogicIT/101-linux-certificates-git tutorial
cd tutorial
sudo git checkout git-example
```

Edit `index.html` and edit the `WELCOME MESSAGE HERE` text. 

```bash
sudo vim index.html
```

## Set up a virtual host on port 80

Create our application file

```bash
sudo cp tutorial.conf /etc/nginx/sites-available/
cd /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/tutorial.conf
```

Verify the content of `tutorial.conf`.

```bash
cat /var/www/tutorial/tutorial.conf
```

```ini
server {
       listen 80;
       listen [::]:80;

       server_name tutorial.example.com;

       root /var/www/tutorial;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```

Copy the cert and private key to the directory where nginx expects them to be, based on our tutorial.conf file. 

```bash
sudo cp ~/myapp-cert.pem /etc/nginx/
sudo cp ~/myapp-key.pem /etc/nginx/
```

Since nginx will need to use the private key, and your private key was secured with a passphrase, add a decrypted version of the key to the `/etc/nginx` folder. 

First, create a copy of the encrypted key. 

```bash
sudo cp /etc/nginx/myapp-key.pem /etc/nginx/myapp-key.pem.enc
```

Unencrypt the private key. Enter the passphrase for the key. 

```bash
sudo openssl rsa -in /etc/nginx/myapp-key.pem.enc -out /etc/nginx/myapp-key.pem
```

Restart the NGINX service

```bash
sudo service nginx restart
```

Test the application

```bash
curl http://localhost
```

Now you can connect to the application on port 80

# justin help, do we do this or go back and fix the cert before

```bash
curl --resolve myapp.domain.com:443:127.0.0.1 https://myapp.domain.com
```