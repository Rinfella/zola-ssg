# Markdown Blog

## This is a static site generated using [zola](https://getzola.org)

Zola is a Static Site Generator built using rust.
It is a CLI based tool that can be used to scaffold our static site easily.
It uses Django and Jinja-like templating.

## Zola Documentation

[Zola Docs](https://www.getzola.org/documentation/getting-started/overview/)

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

There are also many pre-defined themes you can use or build your own theme:
[Zola Themes](https://www.getzola.org/themes/)
