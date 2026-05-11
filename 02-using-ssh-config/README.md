# SSH Through a Jumpbox

## Initial SSH Config

So far, you had to specify the VM IP when connecting via SSH. To simplify this process, and be able to SSH to the VMs using simple names, we will set hosts in the SSH config file. 

Create or edit the SSH config file on your workstation. 

```bash
vi ~/.ssh/config
```

Add the following lines

```text
Host webserver
        HostName {webserver IP}
        User pluser
        IdentityFile ~/.ssh/id_ed25519
```

- `Host`: the name (or list of names) that we will be able to use to ssh to the host. 
- `HostName`: what host to connect to. We will identify the host by their IP. 
- `User`: the user to connect as
- `IdentityFile`: the SSH key to use when connecting to the host. 

Save and exit the file. 

From your workstation, test your new SSH configuration. 

```bash
ssh webserver
```

You are now able to ssh to the `webserver` host without specifying the IP address every time. 

On your workstation, edit the SSH config file to add the following lines. 

```text
Host pki-server
        HostName {pki-server IP}
        User pluser
        IdentityFile ~/.ssh/id_ed25519
```

Make sure that you are now able to SSH to the `pki-server`.

```bash
ssh pki-server
```

## Jumpbox ssh configuration

Edit the SSH config file on your workstation. 

```bash
vi ~/.ssh/config
```

Change the webserver entry to also contain `jumpbox` on on the host line. 

```text
Host webserver jumpbox
        HostName {webserver IP}
        User pluser
        IdentityFile ~/.ssh/id_ed25519
```

Hosts can have several different names assigned to them, in a space separated list. The indented lines for each host are different configs that have been set for each. 

## SSH to private host

As you saw earlier, the `private` host is not able to be accessed from `workstation`. It can only be connected to from either the `webserver` or `pki-server` VMs. 

### GET MORE FROM JUSTIN: something here about realistically there'd be several layers, different zones, policies on what can access what, and you can set all that jumping around in your ssh config.  

Login to the `webserver` host, and from there try to SSH to the `private` host. We will use the `webserver`'s second name, `jumpbox`, to login to it. 

```bash
ssh jumpbox
ssh {private IP}
```

This time, you will be able to connect. 

Instead of always having to first connect to an intermediate VM, we want to be able to SSH from the `workstation` straight to the `private` VM. 

To do this, return to the `workstation` host and run the following command

```bash
ssh -J jumpbox {private IP}
```

- `-J jumpbox`: This tells SSH to use `jumpbox` as a jump host, and will first establish a connection to `jumpbox`, then following that, one to `private`. 

## SSH Config with ProxyJump

Instead of having to specify a jump host every time we want to connect to the `private` VM, we can instead add this information to the `~/.ssh/config` file. 

Modify the ssh config file on your workstation again

```bash
vi ~/.ssh/config
```

Add the information to SSH to the server in the private subnet. This time we're going to add the extra `ProxyJump` attribute

```bash
Host private
        HostName {private IP address}
        User pluser
        ProxyJump jumpbox
        IdentityFile ~/.ssh/id_ed25519
```

Now you can connect to a server easily through the jumpbox without needing to specify anything

```bash
ssh private
```

Now that you can connect to the VM:

- Change the hostname
- Add your public key to the host
- Disable password authentication 
