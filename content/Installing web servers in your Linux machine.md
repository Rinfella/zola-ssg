+++
title = "Installing web servers in your Linux machine"
date = 2024-08-01

[taxonomies]
categories = ["Web Server", "Linux"]
tags = ["Apache", "Nginx", "OpenLitespeed"]
+++

Basic instructions on how to install and configure a couple of web servers in Debian and Debian based Linux distros. Nginx, Apache, OpenLitespeed..

<!-- more -->

We will first look into Apache web server, how to install and configure it:

# Apache

On Debian and its family, Apache web server is by default present in the `apt` repository by the name `apache2`.
You can install it by running:

```bash
sudo apt install apache2 -y
```

By default, on Debian based systems, the configuration files for Apache locates under `/etc/apache2` every modifications you need to do must be done here.

All of your virtual host configs must be inside `/etc/apache2/sites-available` and it must be a `.conf` file.

Example config file:

```xml
<VirtualHost *:80>
        ServerAdmin admin@example.com
        ServerName example.com
        DocumentRoot /var/www/api/public

        <Directory /var/www/api/public>
                AllowOverride All
                allow from all
                Options +Indexes
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

If you want to enable them, run :

```bash
a2ensite your_config.conf
```

This `a2ensite` command will create a symlink of your config fil ejjjjj into the `/etc/apache2/sites-enabled` directory of your Apache config.
You must enable only the config you want to use. You can disable unwanted config files by running:

```bash
a2dissite your_config.conf
```

You can also enable other Apache modules by running the command: `a2enmod module_name`.
Examples:

```bash
a2enmod rewrite
a2enmod proxy
```

You need to restart your `apache2` service every time you modify a configuration.
You can do this by running:

```bash
sudo systemctl restart apache2
```

# Nginx
Like Apache, you can also install Nginx by running one command:

```bash
sudo apt install nginx -y
```

Nginx is a very versatile software, it is not only a web server, but can also act as a load balancer and also as a reverse proxy.
I will not go into details how you can configure it like one, I will leave the rest for you to study further.

Like Apache, the default configurations for Nginx is located at `/etc/nginx` directory and you may make changes for `nginx` here.

All your available sites must be under sites-available and if you want to enable any of them, you must manually symlink them to the sites-enabled directory.

Unlike Apache, it does not have the built-in command like `a2ensite`.
You have to manually do it like so:

```bash
sudo ln -s /etc/nginx/sites-available/myconfig /etc/nginx/sites-enabled
```

Here, `myconfig` is the name of your config file. Unlike Apache, it does not have to be a `.conf` file and the format of these configs are totally different.

Example nginx config file:

```nginx
server {
    listen 80;
    index index.php index.html;
    root /var/www/public;

    # serve static files directly
	location ~* \.(jpg|jpeg|gif|css|png|js|ico|html)$ {
		access_log off;
		expires max;
		log_not_found off;
	}

	# removes trailing slashes (prevents SEO duplicate content issues)
	if (!-d $request_filename)
	{
		rewrite ^/(.+)/$ /$1 permanent;
	}

	# enforce NO www
	if ($host ~* ^www\.(.*))
	{
		set $host_without_www $1;
		rewrite ^/(.*)$ $scheme://$host_without_www/$1 permanent;
	}

	# unless the request is for a valid file (image, js, css, etc.), send to bootstrap
	if (!-e $request_filename)
	{
		rewrite ^/(.*)$ /index.php?/$1 last;
		break;
	}

	location / {
		try_files $uri $uri/ /index.php?$query_string;
	}

	location ~* \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
		deny all;
	}
}
```