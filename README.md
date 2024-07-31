# Markdown Blog

## This is a static site generated using [zola](https://getzola.org)

Zola is a Static Site Generator built using rust.
It is a CLI based tool that can be used to scaffold our static site easily.
It uses Django and Jinja-like templating.

## Zola Documentation

[Zola Docs](https://www.getzola.org/documentation/getting-started/overview/)

## Installing zola CLI

Zola installation can be different based on your OS or Distro.
Take a look at [Zola Installation Docs](https://www.getzola.org/documentation/getting-started/installation/) for more details on how to install.

### MacOs

```bash
brew install zola
```

### Arch Linux

```bash
pacman -S zola
```

### Alpine Linux

```bash
apk add zola
```

### Debian/Ubuntu

For Debian and Debian based distros, visit this [github link](https://github.com/barnumbirr/zola-debian/releases/tag/v0.19.1-1) and download the `.deb` file.
Install the downloaded `.deb` file by using a GUI package installer or by running the command:

```bash
sudo dpkg -i <your_downloaded_zola.deb_file>
```

### Flatpak

```bash
# Install
flatpak install falthub org.getzola.zola

# Run command
flatpak run org.getzola.zola

# Make an alias as "zola" in your `.bashrc/.zshrc` file
alias zola="flatpak run org.getzola.zola"
```

I have mentioned only the popular ways to install in this README.
For more details, please visit the documentation link above.

## Initialization

Run the following command to initialize a zola project:

```bash
zola init <project_dir_name>
```

After running the above command, zola will scaffold a project directory that will look something like this:

```bash
├── config.toml
├── content
├── sass
├── static
├── templates
└── themes

```

You can now start building from this zola boilerplate.
For reference, your project directory structure should look something like this:

```bash
├── config.toml
├── content/
│   └── blog/
│       ├── _index.md
│       ├── first.md
│       └── second.md
├── sass/
├── static/
├── templates/
│   ├── base.html
│   ├── blog-page.html
│   ├── blog.html
│   └── index.html
└── themes/

```

All your markdown `(.md)` page must be inside the `content` directory.
Any sub-directories inside the `content` diractory will be treated as a sub-path,
you will have to specify your routes in your `config.toml`.

### Running/building the project

You can run the local `dev` environment by running:

```bash
zola serve
```

For production, you may want to build your assets:

```bash
zola build
```

There are also many pre-defined themes you can use or build your own theme:
[Zola Themes](https://www.getzola.org/themes/)
