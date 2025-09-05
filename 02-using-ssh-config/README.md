# SSH Through a Jumpbox using config files


We will be using 3 machines for this exercise: 
- Your Workstation
- Jumpbox
- Server


## Prerequisites

Verify if we can ssh from your workstation to the jumpbox (server2), and then from the jumpbox to the server. This will create the .ssh directory at the same time
```bash
ssh [{username}@]jumpbox

ssh pluser@192.169.0.109
```

```bash
ssh [{username}@]server

ssh 192.169.0.110
```

## Rename the VMs for simplicity

On each VM rename them to match `workstation`, `server1`, `server2`

```bash
hostnamectl hostname workstation
cat /etc/hostname
```
You will need to logout and log back in to see the prompt change.

## SSH Config

### Jumpbox ssh configuration
Create or edit the SSH config file on your workstation
```bash
vi .ssh/config
```

Add the following lines 
```bash
Host jumpbox
        HostName {jumpbox ip address}
        User {jumpbox username}
```
example:
```bash
Host jumpbox
        HostName 192.168.0.109
        User pluser
```

From your workstation, test the new configuration
```bash
ssh jumpbox
```

### Server ssh configuration

Modify the ssh config file on your workstation again
```bash
vi .ssh/config
```

Add the information to SSH to the server in the private subnet. This time we're going to add the extra `ProxyJump` attribute

```bash
Host {server connction name}
        HostName {server ip address}
        User {server username}
        ProxyJump {jumpbox}
```

example:
```bash
Host server3
        HostName 192.168.0.110
        User pluser
        ProxyJump jumpbox
```

Now you can connect to a server easily through a jumpbox without needing to specify anything

```bash
ssh server3
```
