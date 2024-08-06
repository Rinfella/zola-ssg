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
Now open the `sshd_config` file using a text editor of your choice: vim, nano, etc.
## Port

You can tell the SSH server to listen to another port by changing the default value of the `Port` variable:

```conf
Port 22
```

You can change this port number into any port you want.
but please make sure that you are not conflicting the other services which also needs the same port.
For example, `mysql` uses port `3306` so you cannot use port `3306` for SSH if you want to install `mysql` in that server.

## ListenAddress

You can tell the SSH server to listen for a specific IP address when attempting to make SSH connections. The default config is:

```conf
ListenAddress 0.0.0.0
```

By listening to `0.0.0.0` it means: by default, the SSH server listens for any SSH connection request coming from any IPv4 address.
Means it allows anyone to attempt to connect to this SSH server.
Even though other people may not know your SSH credentials , it is a good practice to allow only a limited no. of IP addresses to connect to your SSH server.

You can also specify allowed list of IPv6 addresses by using the same `ListenAddress` variable. The default value is `::`, meaning it allows anyone to attempt connection.

## Authentication

### Allow or deny a user or group of users

- DenyUsers
- AllowUsers
- DenyGroups
- AllowGroups

There are certain keywords which allows or denies a user or a user gorup. They are in the following order of priority:

```
DenyUsers > AllowUsers > DenyGroups > AllowGroups
```

In the above hierarchy, `DenyUsers` has the highest priority, it consists of users whose name has or starts with the specified pattern.
Like wise, you can set a number of allowed or denied users or groups based on your needs.
`DenyUsers` has the highest priority while `AllowGroups` has the lowest priority.

### Authorized Keys

We have `AuthorizedKeysFile` variable, which specifies the file that contains the keys which users will be using for authentication.
