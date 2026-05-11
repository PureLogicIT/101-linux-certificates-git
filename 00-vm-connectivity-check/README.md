# VM Connectivity Check

We will be using 4 VMs:

- workstation
- webserver
- pki-server
- private

## Prerequisite

First, verify that we ssh from your laptop to the workstation VM. This will also create the `.ssh` directory at the same time. 

```bash
ssh pluser@{workstation IP}
```

Once connected to the `workstation` VM, make sure you can SSH to both the webserver and pki-server hosts. 

```bash
ssh pluser@{webserver IP}
```

Once connected, exit and SSH to `pki-server`. This time, do not specify the `pluser` as part of the SSH command. 

```bash
ssh {pki-server IP}
```

By default, SSH attempts to authenticate using your current local username. Since you are logged in as `pluser` on your workstation, you can simply run `ssh {pki-server IP}` and the it's assumed you are connecting as pluser.

From your `workstation`, try to SSH to the `private` VM.

```bash
ssh {private vm IP}
```

The connection will not be successful, as the `private` host cannot be accessed directly from the `workstation` VM. In a later section, we will go over how to connect to this host. 

## Change VM hostnames

Now that you have made sure all VMs are accessible from the workstation VM, we will now change the hostnames to match how we are referring to the hosts. 

On the `workstation` VM, change the hostname

```bash
sudo hostnamectl hostname workstation
```

To check if the hostname was successfully changed

```bash
cat /etc/hostname
```

This should now output `workstation`. 

Exit the VM and login again to see the updated hostname in your terminal prompt. 

Make the same change on `webserver` and `pki-server`. 

```bash
sudo hostnamectl hostname webserver
```

```bash
sudo hostnamectl hostname pki-server
```

Verify that the hostname was successfully changed with `cat /etc/hostname` or by logging out and back in to see the prompt change. 

`workstation`, `webserver`, and `pki-server` can now all be accessed without an IP address, and have all had their hostnames changed. 
