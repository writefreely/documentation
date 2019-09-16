# Docker

## Using Docker for Development

The following sections will help you get up and running quickly using Docker.

### Quick setup - Using SQLite

The easiest way is to configure the server to use SQLite, you will be prompted during the interactive call to "--config".

First we configure writefreely and generate some keys.
```shell
docker run -it --volume=data:/home/writefreely writeas/writefreely:latest "--config"
docker run --volume=data:/home/writefreely writeas/writefreely:latest "--gen-keys"
```

To run as a daemon:
```shell
docker run -d -p 8080:8080 --volume=data:/home/writefreely writeas/writefreely:latest
```
To run as a process to watch logs:
```
docker run -p 8080:8080 --volume=data:/home/writefreely writeas/writefreely:latest
```

This is using a Docker `volume` which allows the data generated to persist across multiple instances of a container.

### Custom Builds

If you want to try out your changes in a container, you can use the above instructions after building a new image. When you build the image with our `Dockerfile`, docker will copy your local source files and build the application.

It is best to tag this image with something to make it easier to reference in your `run` commands.

As an example, we could tag an image with our name and then the version with our feature's branch name:
```shell
docker image build -t your_name/writefreely:v0.10.0-branchname .
```
> Remember the `.` at the end of that command, that tells Docker to build in the current context. Which means look for a Dockerfile and other files in the current directory.

Then if you already ran the setup above, you can just run your new container with the same volume:
```shell
docker run -d -p 8080:8080 --volume=data:/home/writefreely your_name/writefreely:v0.10.0-branchname
```

#### Front End Developers

If you would like to work on the front end but are not interested in setting up a Go environment, you can map a few other directories to your local copy of the source repository. Note: that this command does not run the container as a daemon. This makes it easier to stop the running container ( CTRL+C ), for the template changes to take affect you must stop the server and restart.

> Notes: This assumes a host environment with a bash shell or similar. If you want to run detached (in the background) you can add the `-d` flag to the below command.

Run the following from the root of the project repository:
```shell
docker run -p 8080:8080 --volume=data:/home/writefreely \
--volume=$PWD/pages:/home/writefreely/pages \
--volume=$PWD/templates:/home/writefreely/templates \
--volume=$PWD/static:/home/writefreely/static \
writeas/writefreely:latest
```

### Compose Setup - Using SQL

The `docker-setup.sh` script will present you with a few questions to set up your instance. You can hit enter for most of them, except for "Admin username" and "Admin password." The database takes a few seconds to finish starting up the first time so you should wait a few seconds at the beginning of the interactive configuration.

```
./docker-setup.sh
docker-compose up -d
```

Now you should be able to navigate to http://localhost:8080!

When you're done working, you can run `docker-compose down` to destroy your virtual environment, database and configuration data will persist on the docker volumes.

## Using Docker for Production

While we do not currently have a production ready guide, you can use the above compose setup to get up and running on a live server.