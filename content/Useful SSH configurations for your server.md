# Introduction

This article is about the basic SSH server and client configurations for easier and more secure use of the SSH service in general.
I will be using `open-ssh` for this one.
SSH service is the service we used to connect to remote server via terminal.
You can also connect to other machines within the same network.
I will not go into details what a SSH server is.
All we have to know is that it is a `client-server` architecture.
We need to configure both the server and the client to communicate properly.
We will cover basic server configurations first..

# SSH Server

Let's assume we have a cloud server where we host a website.
SSH is most common way we connect to cloud servers.
We need to secure our connection so that our data may not be compromised.
Now let's take a look into the configuration files for the SSH server:
For Debian and Debian based distros, the default location for SSH config files is `/etc/ssh/`
There you can find both `ssh_config` and `sshd_config`.
`ssh_config` is for the client configuration, and `sshd_config` is for the server configuration.
We care about only the `sshd_config` file because this machine will be the server.
Now open the `sshd_conf`
## Port

