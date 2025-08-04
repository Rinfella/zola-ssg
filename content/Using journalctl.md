+++
title = "Using journalctl to troubleshoot Linux"
date = 2025-08-04

[taxonomies]
categories = ["Ubuntu", "Linux", "Debian", "Logs"]
tags = ["troubleshooting", "linux", "journal", "logs"]
+++

This article is about using `journalctl` to troubleshoot Linux systems, such as Ubuntu or other Debian based distros that uses `systemd`.

<!-- more -->

# What is `journalctl`?

`journalctl` is a command-line utility that allows you to query and display messages from the systemd journal, which is a component of the systemd system and service manager. The journal collects logs from various sources, including the kernel, system services, and user applications.

It is a centralized logging system that provides a unified way to access logs, making it easier to troubleshoot issues on Linux systems.
The journal is implemented with the `journald` service, which is a daemon that runs in the background and collects and stores logs from various sources.

# Key Features of `journalctl`:

- **Structured Logging**: The journal stores logs in a structured format, allowing for more efficient querying and filtering.
- **Persistent Storage**: By default, the journal stores logs in memory, but it can be configured to store logs on disk for persistent storage.
- **Rich Metadata**: Each log entry includes metadata such as timestamps, process IDs, user IDs, and more, making it easier to understand the context of each log entry.
- **Filtering and Searching**: `journalctl` provides powerful filtering and searching capabilities, allowing you to find specific log entries based on various criteria.

# Basic Usage of `journalctl`:

To use `journalctl`, you can run the command in your terminal. Here are some basic commands to get you started:

```bash
journalctl
```

The above line will give you tons of lines to scroll through, since it displays the entire journal for your system.
You can use the `less` command to make it easier to navigate:

```bash
journalctl | less
```

You can also pass the `--utc` flag to display the log entries in UTC time:

```bash
journalctl --utc 
```

Journal is very similar to `syslog` logging format, but it contains more than a regular `syslog` file. It includes logs from the early boot process, the kernel, initrd and application standard errors and outputs.

# Filtering Logs with `journalctl`:

You can filter logs based on various criteria using `journalctl`. Here are some common filtering options:

- Show logs from the current boot session:

```bash
journalctl -b
```

- Show list of boot sessions:

```bash
journalctl --list-boots
```
Example output of the above command:

```
-5 0b085f3d1f074f0ea58acd7332651198 Wed 2025-07-30 18:59:57 IST Thu 2025-07-31 18:05:21 IST
-4 ead24dcca58048df8cfc56bdd77675e8 Thu 2025-07-31 21:51:56 IST Thu 2025-07-31 23:01:26 IST
-3 717590cf734542a78263adde4bae9fc8 Thu 2025-07-31 23:02:00 IST Fri 2025-08-01 10:08:49 IST
-2 97d43a3517c544398ae83f41357c6628 Fri 2025-08-01 10:45:12 IST Fri 2025-08-01 12:37:31 IST
-1 0d5a824493884097879fdf0504c4e669 Fri 2025-08-01 12:38:05 IST Fri 2025-08-01 19:24:11 IST
0  e15c9f6721554138bc4e527a938ca8af Fri 2025-08-01 19:57:22 IST Mon 2025-08-04 11:44:39 IST
```

- Show logs from the previous boot session:

```bash
journalctl -b -1
```

In the above example boot sessions, `0` is my current boot session, `-1` is the previous boot session, `-2` is the one before that, and so on.
So, we just pass the `-b` flag followed by the boot session number to get the logs from that specific boot session.

# Persistent Storage of Logs with `journalctl`

By default, the journal stores logs in memory, which means that logs are lost after a reboot. To enable persistent storage of logs, you must explicitly define it in your settings and create a directory for it.

This is the default location for persistent logs: `/var/log/journal/`

```bash
sudo mkdir -p /var/log/journal
```

By creating this directory, systemd is smart enough to persist log storage if the `journal` directory exists inside `/var/log/`.
Or, if you want to make sure the logs are always stored in persistent mode, you can define it in the systemd configuration file:

```bash
sudo vim /etc/systemd/journald.conf
```

> You can use any text editor of your choice to edit this file.

And then you must uncomment the line that says `Storage=auto` in the `[Journal]` section.
Change the value of `Storage` from `auto` to `persistent` to enable persistent storage of logs.

```conf
[Journal]
Storage=persistent
```
After making this change, save the file and restart the `journald` service to apply the changes:

```bash
sudo systemctl restart systemd-journald
```
This will ensure that logs are stored on disk and are available even after a reboot.

# Another Advanced method to filter logs:

You can also filter logs based on specific criteria, such as the service name, priority level, or time range. Here are some examples:

- Show logs for a specific service (e.g., `sshd`):

```bash
journalctl -u sshd
```

You can also chain them with other filter flags like `--since` and `--until` to filter logs for a specific service within a time range:

```bash
journalctl -u sshd --since "yesterday" --until "1 hour ago"
```


- Show logs with a specific priority level (e.g., `error`):

```bash
journalctl -p err
```

- Show logs within a specific time range:

```bash
journalctl --since "2025-08-01 12:00:00" --until "2025-08-01 14:00:00"
```

You can use the `--since` flag and `--until` flag separately to specify the start and end times. 
Or you can use the `--after` and `--before` flags to specify the start and end times.

The journal also understands some relative values and named shortcuts.
For instance, you can use the words `today`, `yesterday`, `tomorrow`, `now`, and so on.

```bash
journalctl --since "yesterday"
journalctl --since "yesterday" --until "1 hour ago"
journalctl --since 09:00 --until "1 hour ago"
```

- Filtering logs by PID, UID or GID
You can also filter logs by process ID (PID), user ID (UID), or group ID (GID):

```bash
journalctl _PID=1234 
journalctl _UID=1000
journalctl _GID=1000
```

You can take a look at a users's ID by using the following command:

```bash
id -u www-data
```

You can use your own user ID or any other user ID to filter logs for that specific user.
Then you can do something like this:

```bash
journalctl _UID=1000 --since "yesterday"
```

- View logs by executable path:

```bash
journalctl /usr/bin/bash
```

- Viewing kernel logs:

```bash
journalctl -k
```

All these commands are valid and journal can understand these.

# Customizing `journalctl` Output

You can customize the output of `journalctl` by adding additional flags and options. Here are some examples:

- Show logs in a specific format (e.g., JSON):

```bash
journalctl -o json
journalctl -o json-pretty
```

- Show logs with a specific output format (e.g., short):

```bash
journalctl -o short
```

- Show logs with a truncated output:

```bash
journalctl --no-full
```

- Display all informations:

```bash
journalctl -a
```

- Disable pager in journalctl output:

```bash
journalctl --no-pager
```

- Monitor live systemd logs with `journalctl`:

```bash
journalctl -n
journalctl -n 100
```

Passing the number 100 after `-n` will show the last 100 lines of logs.

- Actively monitoring logs in real-time:

```bash
journalctl -f
```

# Managing `journalctl` Logs and Disk Usage:

```bash
journalctl --disk-usage
```

The above command shows how much space the `journalctl` logs are taking up on your system.

- Deleting old logs:

```bash
journalctl --vacuum-time=2d
```

The above command will remove all logs older than 2 days.

```bash
journalctl --vacuum-size=1G
```

And then this command will shrink all the logs in the journal to 1GB.

# Configuring Disk space limit for `journalctl` logs:

You can configure the maximum disk space that `journalctl` logs can use by editing the `/etc/systemd/journald.conf` file.

```bash
sudo vim /etc/systemd/journald.conf
```

You can set the following parameters to limit the disk space used by the journal:

- `SystemMaxUse=`: Specifies the maximum space the journal files can occupy in the disk.
- `SystemKeepFree=`: Specifies the amount of disk space that should be kept free for other uses.
- `SystemMaxFileSize=`: Specifies the maximum size of individual journal files.
- `RuntimeMaxUse=`: Specifies the maximum space the journal files can use during runtime (in the `/run` filesystem).
- `RuntimeKeepFree=`: Specifies the amount of disk space that should be kept free during runtime (in the `/run` filesystem).
- `RuntimeMaxFileSize=`: Specifies the maximum size of individual journal files during runtime (in the `/run` filesystem) before being rotated.

By setting the above parameters in you journald config, you can manage how your logs are retained in the system.
Keep in mind that `SystemMaxFileSize` and `RuntimeMaxFileSize` will target archived files since the journalctl logs are rotated and archived after a certain period of time.

# Troubleshooting `journalctl`:

If you encounter issues with `journalctl`, here are some common troubleshooting steps:

- **Check the status of the `journald` service**:

```bash
systemctl status systemd-journald
```
- **Check disk usage of the journal**:

```bash
journalctl --disk-usage
```

- **Running `journalctl` as root**:
If you are unable to view certain logs, you may need to run `journalctl` with root privileges:

```bash
sudo journalctl
```

- **Adding your user to journal group**:

You can add your user to the `systemd-journal` group to allow access to the journal without using `sudo`:

```bash
sudo usermod -aG systemd-journal $USER
```

- **Giving appropriate permissions to journal directory**:

You must give appropriate permissions to the journal directory to allow access to the journal logs:

```bash
sudo chown root:systemd-journal /var/log/journal
sudo chmod 2755 /var/log/journal
```

# Debugging a failed system service:

If a system service fails to start or behaves unexpectedly, you can use `journalctl` to debug the issue. Here’s how:

1. **Check the status of the service**:

```bash
sudo systemctl status nginx
```

2. **Check journal logs for that service**:

```bash
journalctl -u nginx
```

3. **Checking logs from the current boot**:

```bash
journalctl -u nginx -b
```

4. **Checking logs from a specific time frame**:

```bash
journalctl -u nginx --since "10 minutes ago" --until "now
```

5. **Enable verbose output or extended error messages**:

```bash
journalctl -xe
```

6. **Extracting specific information from the logs**:

You can pipe the output of journal to `grep` in order to select specific keywords

```bash
journalctl -u sshd.service | grep "Failed"
```

# Conclusion

The above mentioned are the basic usage of `journalctl` command.
This is a very helpful CLI tool since we can view centralized logs.
Since `journalctl` comes with `systemd`, you need to use `systemd` as your init system.
All log files are maintained and rotated by journal regularly.
To know more about `journalctl` visit [journalctl-manpage](https://man7.org/linux/man-pages/man1/journalctl.1.html)
[Journald-manpage](https://man7.org/linux/man-pages/man8/systemd-journald.service.8.html)
