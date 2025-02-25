# Statik

[![Build Status](https://travis-ci.org/thanethomson/statik.svg?branch=master)](https://travis-ci.org/thanethomson/statik)
[![PyPI](https://img.shields.io/pypi/v/statik.svg)](https://pypi.python.org/pypi/statik)
[![PyPI](https://img.shields.io/pypi/pyversions/statik.svg)](https://pypi.python.org/pypi/statik)

## Overview
**Statik** aims to be a simple, yet powerful, generic static web site generator.
Instead of forcing you to adhere to a particular data structure/model (like the
standard blog data model, with posts, pages and tags), **Statik** allows you to
define your own data models in YAML format, and instances of those data models
either in YAML or Markdown. This is all loaded into an in-memory
[SQLAlchemy](http://www.sqlalchemy.org/) SQLite database when rendering your
*views*.

Then, code up your templates using the [Jinja2](http://jinja.pocoo.org/)
templating engine (very similar to the Django templating engine), or
[Mustache](http://mustache.github.io/) templates.

Finally, define your *views* (either complex or simple) in YAML format, telling
**Statik** how to render your data and templates to specific URLs for your shiny
new static web site. Write queries for your views in SQLAlchemy's [ORM
syntax](http://docs.sqlalchemy.org/en/rel_1_0/orm/tutorial.html), or
[MLAlchemy](https://github.com/thanethomson/MLAlchemy) to make your life easier.

See the [wiki](https://github.com/thanethomson/statik/wiki) for more details.

## Requirements
In order to install **Statik**, you will need:

* Python 3.6 -> 3.9
* `pip`
* or `uv`

## Installation
Simply run the following:

```bash
> pip install https://github.com/blackdaemon/statik.git
```
To upgrade, just run the following:

```bash
> pip install --upgrade https://github.com/blackdaemon/statik.git
```

## Usage
To build the project in the current directory, writing output files to the
`public` directory within the current directory:

```bash
> statik
```

To build a project in another directory, writing output files to the `public`
directory within *that* directory:

```bash
> statik -p /path/to/project/folder
```

To build a project in another directory, with control over where to place the
output files:

```bash
> statik -p /path/to/project/folder -o /path/to/output/folder
```

## Project QuickStart
To create an empty project folder with the required project structure, simply
run:

```bash
> statik -p /path/to/project/ --quickstart
```

## Statik Projects
A **Statik** project must adhere to the directory structure as follows:

```
config.yml    - YAML configuration file for the overall project.
assets/       - Static assets for the project (images, CSS files,
                scripts, etc.). Ignored if themes are used.
data/         - Instances for each of the different models, defined either in
                YAML or Markdown format.
models/       - A folder specifically dedicated to model definitions, in YAML
                format.
templates/    - Jinja2/Mustache template files. Ignored if themes are used.
templatetags/ - Python scripts defining custom Jinja2 template tags and
                filters.
themes/       - If your project uses themes, place them here. Each theme
                must be uniquely named, and must contain an "assets"
                and "templates" folder.
views/        - Configuration files, in YAML format, defining "recipes" for how
                to generate various different URLs (which models to use, which
                data and which templates).
```

For example projects, see the `examples` directory in the source repository.
For more information, see the
[wiki](https://github.com/thanethomson/statik/wiki).

## Themes
Themes for **Statik** will slowly start appearing in the [Statik
Themes](https://github.com/thanethomson/statik-themes) repository. Watch that
space!

## Remote upload
**Statik** can publish your generated site for you through SFTP or through
[Netlify](https://netlify.com).

### SFTP

To publish your website via SFTP, you can configure certain values by way of
configuration file or environment variable-based options. Some sensitive
options, however, must be configured exclusively by way of environment
variables.

For your configuration file:

```yaml
deploy:
  sftp:
    host: your-server.com
    dest-path: /folder/on/your/server/
    user: youruserforyourserver

    # you can also optionally specify which SSH private key to use
    key-file: ~/.ssh/my-private-key

    # optionally specify SFTP port (default: 22)
    port: 2222
```

Set up your sensitive environment variables prior to running Statik:

```bash
# If your server requires password-based authentication
> export SFTP_PASSWORD=mysftppassword

# If your SSH key file is encrypted and it requires a password to decrypt
> export SFTP_KEY_FILE_PASSWORD=mysftpkeyfilepassword
```

Alternatively, you could set up all SFTP variables through environment variables
alone:

```bash
> export SFTP_HOST=your-server.com
> export SFTP_PORT=2222                  # optionally set SFTP port
> export SFTP_DEST_PATH=/folder/on/your/server
> export SFTP_USER=youruserforyourserver
> export SFTP_PASSWORD=mysftppassword
> export SFTP_KEY_FILE=~/.ssh/my-private-key
> export SFTP_KEY_FILE_PASSWORD=mysftpkeyfilepassword
```

Then run Statik using the following deployment command:

```bash
> statik --deploy sftp
```

For troubleshooting SFTP connections, run this using the `-v` flag to increase
output verbosity:

```bash
> statik --deploy sftp -v
```

### Netlify

To publish your website via Netlify, you will need 2 things: your Netlify access
token and your Netlify site ID.

First, specify your access token and site ID as environment variables:

Linux:

```bash
> export NETLIFY_AUTH_TOKEN=<netlify_token>
> export NETLIFY_SITE_ID=<netlify_site_id>
```

Windows

```bash
> set NETLIFY_AUTH_TOKEN=<netlify_token>
> set NETLIFY_SITE_ID=<netlify_site_id>
```

Then, run **Statik** by passing in `--deploy=netlify`.

```bash
statik --deploy netlify
```
**Statik** will upload the static site that it outputs.

## Building statik


To build the `statik` project:

```bash
> uv sync
> uv build
> statik
```


## License
**The MIT License (MIT)**

Copyright (c) 2016-2019 Thane Thomson

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
