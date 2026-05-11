# Challenge: Install Scribbler-rs

Scribbler is an open source game that replicates Skribbl game. This is an multiplayer drawing and guessing game.

BONUS: if you can create a service for this game and be able to run and stop it.

## Prerequisites

Before start, install Golang at the link below. 

- [Install Golang](https://go.dev/doc/install)

## Instruction

To be able to build and deploy the app you will need to clone the repository:

```bash
git clone https://github.com/scribble-rs/scribble.rs.git
```

Open the git repository in your browser and follow the instructions listed in the project's README file. 

You will need to edit the config so that the app listens on port 80. Then, build the app using the golang engine to create the execuable file to run the application

The config file is under: internal/config/config.go

The main file is under: cmd/scribblers/main.go

**Note**:
You do not need to setup NGINX to connect from the VM.
