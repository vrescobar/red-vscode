# Red Lang template: VSCode dev environment

This repository contains a [Red Language](red-lang.org) hello world script with all the necessary configuration, plugins, and environment for [visual studio code](code.visualstudio.com)

Just clone this repo and open the folder in VSCode (requires docket), to develop GUI please read the instructions bellow.

`git clone https://github.com/vrescobar/red-vscode.git MyNewProject`

## Features

All plugins and docker provide the next features:

* The newest red language with GTK support (daily build)
* The latest stable red language (stable build: 0.6.4)
* Plugin extension recommendations for Visual Studio Code:
  * Red language plugin support for autocompletion and language server
  * Integration with red's REPL to send selected text to the command line
  * Build commands so you can just selecto how to compile or run your code
  * A development Docker File (with a vscode extension to run the build and code on that docker)

* The docker development file includes for seamless development of the Red Language, **independently** of the host system:
  * Support to run red with 32 bit libraries
  * Latest build for Red Lang (rebuild your docker for updates)
  * All necessary GTK libraries for UI development
  * An X server to forward your UI programs to your host system (including windows or mac)
* Build scripts for the

This docker file downloads the latest red language binary precompiled, a daily build post 0.6.4 with GTK support.

See the aliases included at the [alias](.devcontainer/aliases.sh) file and don't forget to customize the [Dockerfile](.devcontainer/Dockerfile).

The development dockerfile functionality is provided through the VSCode plugin look at its [documentation](https://code.visualstudio.com/docs/remote/containers) for more information for a more customized docker environment (highly recommended).

## Instructions

1. git clone this repository
2. Make sure you have [docker](www.docker.com) installed on your system and the docker daemon has started
3. Open the cloned directory in Visual Studio Code
4. If it doesn't happen automatically install the [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension for Visual Studio Code along with all other [recommended extensions](.vscode/extensions.json)

The first time you open the project Visual Studio will ask you to open the project in the docker development environment, accept it and also accept the recommendation to install the recommended extensions.

Also if you modify the Dockerfile or you open it the first time visual studio will trigger a docker build for you.


## The Red Lang versions included

When making the Docker container for development
This docker container will download the latest [red](red-lang.org) language binaries for linux as well as the stable. Both binaries will be included in your system as `red-latest` and `red-064` respectly. A `red` command will be set in path pointing to `red-latest` as this is the only way to run red's GUI at the moment in linux.

## How to open red GUI applications (configuration)

By default you might not be capable of running Red programs with a GUI from the docker container, in order to do that you need to forward docker's X11 server to your host system.

* Linux users probably already have an X11 system, but still should make sure to properly configure everything to work.
* Windows users might to follow a [tutorial](https://dev.to/darksmile92/run-gui-app-in-linux-docker-container-on-windows-host-4kde).
* Mac users will find instructions on how to install an X Server [here](https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc)

Remember that usually **you need to restart your user before using the X server**.

Once your host system is running an X server you have to launch the docker
container with the variable DISPLAY pointing to `ip:0` for example if at your
host system your find out your IP address to be:

```bash
$ ipconfig getifaddr en0
192.168.1.54
```

Then set `192.168.1.54:0` the `DISPLAY` at the [devcontainer](.devcontainer/devcontainer.json)

## How do I open the docker container in a separate terminal

If you want to use a terminal detached from vscode just run:
```
docker run -e DISPLAY=IP_ADDRESS:0 -ti d26b44168c04
```
Assuming that IP_ADDRESS is your local X Server running. If you don't need to display

The docker hash you can find it using `docker ps` or in the bash prompt in the visual studio terminal:

In this example it is `d26b44168c04`:
```
root @ d26b44168c04 : ~ # red --version
0.6.4
```


## How can I manually build the docker file

Just type:

```bash
cd .devcontainer
docker build .
```

However after you make any changes on the docker file or related configuration files, visual studio code (with the docker extension) should ask you on whether you want to rebuild the docker.

## I want to make a contribution or report a bug

Feel free to make a pull request, especially regarding documentation as Red Lang being a new language and platform needs lots of atention in order to become as accesible as possible for beginners.