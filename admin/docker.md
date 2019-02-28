# Docker

## Using Docker for Development

If you'd like to use Docker as a base for working on a site's styles and such,
you can run the following from a Bash shell.

*Note: This process is intended only for working on site styling. If you'd
like to run Write Freely in production as a Docker service, it'll require a
little more work.*

The `docker-setup.sh` script will present you with a few questions to set up
your dev instance. You can hit enter for most of them, except for "Admin username"
and "Admin password." You'll probably have to wait a few seconds after running
`docker-compose up -d` for the Docker services to come up before running the
bash script.

```
docker-compose up -d
./docker-setup.sh
```

Now you should be able to navigate to http://localhost:8080 and start working!

When you're completely done working, you can run `docker-compose down` to destroy
your virtual environment, including your database data. Otherwise, `docker-compose stop`
will shut down your environment without destroying your data.

## Using Docker for Production

WriteFreely doesn't yet provide an official Docker pathway to production. We're
working on it, though!
