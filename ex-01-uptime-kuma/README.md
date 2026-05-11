# Uptime Kuma

To install Uptime Kuma, first install Node.js. Git and Nginx are also required, but they are already installed. 

## Download Node.js

```bash
# Update and install necessary packages
sudo apt update
sudo apt install libatomic1

# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 26

# Verify the Node.js version:
node -v # Should print "v26.1.0".

# Verify npm version:
npm -v # Should print "11.13.0".
```

## Clone Uptime Kuma

Next, clone and setup `Uptime Kuma`. 

```bash
git clone https://github.com/louislam/uptime-kuma.git
cd uptime-kuma
npm run setup
```

Run the `Uptime Kuma` server using node. 

```bash
node server/server.js
```

Load the webpage at `{SOMETHING IP}:3001`. 

Select SQLite, and create an account. 

In the top left, add a new monitor. 

Enter a name for the monitor, and under `URL`, enter the link to the `Scribble.rs` app. 

Change the `Heartbeat Interval` to 5 seconds. 

