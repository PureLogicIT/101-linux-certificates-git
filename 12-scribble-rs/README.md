# Challenge: Install Scribbler-rs

Scribbler is an open source game that replicates Skribbl game. This is an multiplayer drawing and guessing game.

BONUS: if you can create a service for this game and be able to run and stop it.

## Prerequisite:

Here are things you will need to install before attempting the challenge:

 - [Install Golang](https://go.dev/doc/install)
 - [Install Git](https://git-scm.com/downloads/linux) 

## Instruction:

To be able to build and deploy the app you will need to clone the repository:

```
git clone https://github.com/scribble-rs/scribble.rs.git
```
Once done, you will need to edit the config so that the app will listen on port 80. Then you will build the app using the golang engine to create the execuable file to run the application

The config file is under: internal/config/config.go

The main file is under: cmd/scribblers/main.go

### Note:
You do not need to setup NGINX to connect from the VM. 
