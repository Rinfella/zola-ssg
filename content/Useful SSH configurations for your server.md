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
## Authentication Settings

### Port

You can tell the SSH server to listen to another port by changing the default value of the `Port` variable:

```conf
Port 22
```

You can change this port number into any port you want.
but please make sure that you are not conflicting the other services which also needs the same port.
For example, `mysql` uses port `3306` so you cannot use port `3306` for SSH if you want to install `mysql` in that server.

### ListenAddress

You can tell the SSH server to listen for a specific IP address when attempting to make SSH connections. The default config is:

```conf
ListenAddress 0.0.0.0
```

By listening to `0.0.0.0` it means: by default, the SSH server listens for any SSH connection request coming from any IPv4 address.
Means it allows anyone to attempt to connect to this SSH server.
Even though other people may not know your SSH credentials , it is a good practice to allow only a limited no. of IP addresses to connect to your SSH server.

You can also specify allowed list of IPv6 addresses by using the same `ListenAddress` variable. The default value is `::`, meaning it allows anyone to attempt connection.

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
Default location for authorized keys is `~/.ssh/authorized_keys`

### LoginGraceTIme

The server disconnects after this time if the user has not successfully login.
If the value is 0, there is no time limit.
The default value is 120.

### MaxAuthTries

This specifies the max number of authentication attempts permitted per connection.
Once the number of failures reaches half this value, additional failures are logged.
The default is 6.

### MaxSessions

Specifies the number of open sessions permitted per network connection.
The default is 10.

### Max Startups

This specifies the max number of concurrent unauthenticated connections to the SSH daemon.
Additional connections will be dropped until authentication succeeds or the `LoginGraceTime` expires for a connection.
The default is 10.

### PasswordAuthentication

This specifies whether you can connect to the SSH server using password or not.
The default is `yes`;

### PermitEmptyPasswords

This option allows empty password strings to be used when password based authentication is on.
The default value is `'no'` .
You must not enable this for safety purposes even if you enable password authentication.

## Connection Settings

### ClientAliveCountMax

This sets the number of client alive messages even if `sshd` does not send back any messages.
If this threshold is reached while client alive messages are being sent, then `sshd` will disconnect the client, terminating the session.
The default value for `ClientAliveCountMax` is 3.
Let's say if the `ClientAliveInterval` variable is set to 15, then the SSH connection for the SSH client will persist for `3 * 15` seconds . i.e. 45 seconds.


### ClientAliveInterval

This sets a timeout interval in seconds after which if no data has been received from the client.
This indicates how long will the client be connected to the server with being idle.

# SSH Client

You can configure the client side of the SSH too so that whenever you connect to any of your SSH servers, you can have the behaviour you want.
For me personally, I want to persist my connection for as long as an hour or half an hour or so.
To achieve this, I have the follwing configurations in my config file:

## Global Client Config

Contents of `~/.ssh/config` file:
```
Host *
	ServerAliveInterval 60
	ServerAliveCountMax 60
	Compression yes
	Cipers aes128-ctr, aes192-ctr, aes256-ctr
```

In the above configurations, `Host *` defines that the configuration is for all the hosts connected using this SSH client.
The `ServerAliveInterval` variable defines how frequent the client must send a message to the server in order to keep the connection alive.
The `ServerAliveCountMax` variable sets the number of keep alive messages that may be sent by the client without the client receiving any messages back from the server.
Putting both those values to `60` will result in the client sending keep alive request every minute for 60 times.
This means that all SSH connections made using this settings will persist for 1 hour even if the client is idle without receiving back any messages from the server.

## Hosts Config

An example configuration will look something like this:

```conf
Host my-website
	Hostname mydomain.com
	User myuser
	Port 6969
	IdentityFile /path/to/your/keyfile
```

This example configuration defines a simple SSH connection for `mydomain.com` using the firendly name `my-website`.
You can simply SSH into the above server by typing `ssh my-website`.
It will automatically use all the parameters you specify in this config file.
It will SSH into the server of the domain `mydomain.com`, you can also put an IP address instead of a domain name.
The `User` variable is for the server system user.
If you are using password based authentication, then you do not need to add this `IdentitiyFile` parameter.
It will automatically prompt you your password.
If you set up your SSH connection using a key based authentication, it is better to maintain your SSH connection this way rather than passing the key file into the ssh connection string.

