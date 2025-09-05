# Solution: Install scribbler

## 1. install golang

```
// download and unpack golang
wget https://dl.google.com/go/go1.24.4.linux-amd64.tar.gz
sudo tar -C /usr/local/ -xzf go1.24.4.linux-amd64.tar.gz

```

edit .profile file to add path environment variable

```
export PATH=$PATH:/usr/local/go/bin
```

To test run:
```
go version
```

## 2. install git

This should already be done in a previous section, but in case this is the methode of installation

```
apt-get install git
```

## 3. build and execute scribbler

```
git clone https://github.com/scribble-rs/scribble.rs.git
cd scribble.rs
```

edit the /internal/config/config.go and replace the port from 8080 to 80
```
port:80
```

build the app to generate the executable app
```
go build ./cmd/scribblers
./scribler-rs
```