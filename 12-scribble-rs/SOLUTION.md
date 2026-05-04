# Solution: Install scribbler

## 1. install golang

Download and unpack Golang

```bash
wget https://dl.google.com/go/go1.24.4.linux-amd64.tar.gz
sudo tar -C /usr/local/ -xzf go1.24.4.linux-amd64.tar.gz

```

Edit .profile file to add path environment variable

```bash
export PATH=$PATH:/usr/local/go/bin
```

To test run:

```bash
go version
```

## 2. Install git

This should already be done in a previous section, but in case this is the method of installation

```bash
apt-get install git
```

## 3. Build and execute scribbler

```bash
git clone https://github.com/scribble-rs/scribble.rs.git
cd scribble.rs
```

edit the /internal/config/config.go and replace the port from 8080 to 80

```text
port:80
```

build the app to generate the executable app

```text
go build ./cmd/scribblers
./scribbler-rs
```
