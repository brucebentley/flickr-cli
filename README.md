# Flickr CLI

[![Build Status](https://travis-ci.com/brucebentley/flickr-cli.svg?branch=master)](https://travis-ci.com/brucebentley/flickr-cli)

A command-line interface to [Flickr][Flickr Homepage]. Upload & download photos, photo sets, directories via shell.

## Prerequisites

Before you begin, make sure you have the following items installed & available.

- [Flickr Account][Flickr Homepage] **( Required )** — If you wish to use this `flickr-cli` repository
  - [Flickr `API Key`][Flickr App Create] **( Required )** — In order to interact with the Flickr API
- [`Git`][Git Homepage] / [`GitHub`][GitHub Homepage] — So that you may clone and/or fork this repository
- [`Composer`][Composer Homepage] **( Required )** — Dependency Manager for `PHP`
  - [Linux/Unix/macOS Installation Instructions][Composer on Linux/Unix/macOS]
  - [Windows Installation Instructions][Composer on Windows]
- [`Docker`][Docker Homepage] **( Optional )** — If you wish to run this app inside of a Docker Container
  - [Install `Docker` on macOS][Docker on macOS]
  - [Install `Docker` on Windows][Docker on Windows]
  - [Install `Docker` on CentOS Linux][Docker on CentOS]
  - [Install `Docker` on Debian Linux][Docker on Debian]
  - [Install `Docker` on Fedora Linux][Docker on Fedora]
  - [Install `Docker` on Ubuntu Linux][Docker on Ubuntu]
- [GitHub `Personal Access Token`][GitHub Personal Access Token] **( Optional )** — If you plan on using `Docker`

## Installation

1. Clone the `flick-cli` repository from [Github][GitHub Repository]:

   ```sh
   git clone https://github.com/brucebentley/flickr-cli.git
   ```

2. Install all of the required dependencies:
   ```sh
   composer install
   ```

3. Go to <https://www.flickr.com/services/apps/create/apply/> to create a new Flickr API key:  
  
   > &nbsp;  
   > **Please Note:**  
   > The first time you run `./bin/flickr-cli auth` you'll be prompted to enter  
   > your new consumer key and secret.  
   > &nbsp;

## Usage

### Authenticating To The Flickr API

**Get your Flickr access token & authenticate to Flickr:**

```sh
./bin/flickr-cli auth
```

### Uploading To Flickr

```sh
./bin/flickr-cli upload [-d DESCRIPTION] [-t TAG,...] [-s SET,...] DIRECTORY...
```

### Downloading From Flickr

**Basic Usage Example:**

```sh
./bin/flickr-cli download -d DIRECTORY [SET...]
```

**To download all photosets to directory `photosets`:**

```sh
./bin/flickr-cli download -d photosets
```

**To download only the photoset *Holiday 2018*:**

```sh
./bin/flickr-cli download -d photosets 'Holiday 2018'
```

**To download all photos into directories named by Photo ID, you can use the `--id-dirs` option:**

> &nbsp;  
> **Please Note:**  
> This is a perfect method for doing a complete Flickr account backup, being that  
> Photo ID directory names **will not** change when you rename albums or photos.  
> &nbsp;

```sh
./bin/flickr-cli download -d flickr_backup --id-dirs
```

This creates a stable directory structure of the form `destination_dir/hash/hash/photo-ID/`,
and saves the full original photo file along with a `metadata.yml` file containing all photo
metadata. The hashes, which are the first two sets of two characters of the MD5 hash of the ID,
are required in order to prevent a single directory from containing too many subdirectories
_(to avoid problems with some filesystems)_.

## Using Docker

### Docker Image Setup

To use this software within Docker, follow these steps.

1. **Create a volume:** _(This is used to store the configuration file for the `auth` step)_
   ```sh
   docker volume create flickr-cli
   ```

2. **Get the access token:** _(it will create `config.yml` file in the volume)_
   ```sh
   docker run --rm -it -u $(id -u):$(id -g) -v "$PWD":/mnt -v flickr-cli:/data brucebentley/flickr-cli auth
   ```

3. **Alternatively, you can store the `config.yml` in your `$HOME/.flickr-cli` directory and use the following:**

   ```sh
   # CREATE A NEW `.flickr-cli` DIRECTORY IN YOUR USER'S `$HOME` DIRECTORY.
   mkdir $HOME/.flickr-cli

   docker run --rm -it -u $(id -u):$(id -g) -v "$PWD":/mnt -v "$HOME/.flickr-cli":/data brucebentley/flickr-cli auth
   ```

### Docker Image Usage

**Upload directory `2018.12.24-Holiday-2018` full of `JPEG` photos to Flickr:**

```sh
docker run --rm -it -u $(id -u):$(id -g) -v "$PWD":/mnt -v flickr-cli:/data brucebentley/flickr-cli upload --config=/data/config.yml --tags '2018.12.24 "Holiday 2018" Holiday 2018' --sets "Holiday 2018" 2018.12.24-Holiday-2018
```

**For Docker image troubleshooting, you can use:**

```sh
docker run --rm -it -u $(id -u):$(id -g) -v "$PWD":/mnt -v flickr-cli:/data --entrypoint=/bin/bash brucebentley/flickr-cli
```

### Docker Image Paths

- `/app` — Main application directory
- `/data` — Volume for variable data
- `/mnt` — Host system's `$PWD`

## 3rd-Party Documentation

- [Flickr API Documentation][Flickr API Documentation]
- [Docker Documentation][Docker Documentation]

## License

Copyright &copy; 2018 Bruce Bentley <https://brucebentley.io>

This program is free software: you can redistribute it and/or modify it under the terms of the
GNU General Public License as published by the Free Software Foundation, either version 3 of
the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
See the GNU General Public License for more details. You should have received a copy of the
GNU General Public License along with this program. If not, see <http://www.gnu.org/licenses/>.

[Composer Homepage]: https://getcomposer.org
[Composer on Linux/Unix/macOS]: https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos
[Composer on Windows]: https://getcomposer.org/doc/00-intro.md#installation-windows
[Docker on CentOS]: https://docs.docker.com/install/linux/docker-ce/centos
[Docker on Debian]: https://docs.docker.com/install/linux/docker-ce/debian
[Docker on Fedora]: https://docs.docker.com/install/linux/docker-ce/fedora
[Docker on macOS]: https://docs.docker.com/docker-for-mac/install
[Docker on Ubuntu]: https://docs.docker.com/install/linux/docker-ce/ubuntu
[Docker on Windows]: https://docs.docker.com/docker-for-windows/install
[Docker Documentation]: https://docs.docker.com
[Docker Homepage]: https://www.docker.com
[Flickr API Documentation]: https://flickr.com/services/api
[Flickr App Create]: https://www.flickr.com/services/apps/create/apply
[Flickr App Garden]: https://flickr.com/services
[Flickr Homepage]: https://flickr.com
[Git Homepage]: https://git-scm.com
[GitHub Homepage]: https://github.com
[GitHub Personal Access Token]: https://github.com/settings/tokens/new
[GitHub Repository]: https://github.com/brucebentley/flickr-cli
