# Setting up the environment

If you are new to ansible or linux administration, the following steps should get you from zero to frigate.

## Installing the Target OS

1. Install Ubuntu (I use 22.04.3 Server, but should work for any version after 20.04).

2. When partitioning, ensure the root partition has plenty of space or setup `/opt/frigate/media` with a large drive or file system. All of the frigate video files will be stored in that directory.

3. During installation create a user called "frigate" with admin privileges.

## Setting up passwordless SSH

**NOTE:** All of the steps below should be run from your non-root user. Nothing should require sudo or root user on the ansible deployment server.

1. From your ansible system (e.g. WSL or linux terminal) ensure you have a ssh key. You can run `ssh-keygen -t ed25519 -a 100` to create a new one. You can just hit `ENTER` through the prompts and it will generate a new ssh key without passphrase.

2. To copy the new key over to frigate server run `ssh-copy-id frigate@<IP ADDRESS>` 

3. Login to the server with `ssh frigate@<IP ADDRESS>`. You shouldn't need a password anymore. Run `sudo visudo` and input your frigate user password that you setup when installing the server.

```bash
# Example of generating ssh keys and copying them over
ssh-keygen -t ed25519 -a 100
ssh-copy-id frigate@10.0.0.2
ssh frigate@10.0.0.2
sudo visudo
```

4. Navigate to the line below `%sudo   ALL=(ALL:ALL) ALL` and enter `frigate ALL=(ALL) NOPASSWD:ALL`. CTRL-X to exit, saving all files as asked. You should be able to run `sudo whoami` with no password. If everything worked, exit out of the ssh session back to the ansible controller.

```bash
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
frigate ALL=(ALL) NOPASSWD:ALL  # << Insert this
```


From here, you should be able to follow the Quickstart Guide in the README.md doc.