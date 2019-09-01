WPDock is a very basic but certainly usefull docker setup for working with WordPress. It comes with: 

- PHPMyAdmin (on port [8036](http://localhost:8036))
- the `./wpdock` command to run WP-CLI commands directly from the host

## Create a new WordPress site

```sh
## Clone repository
$ git clone git@github.com:kadimi/wpdock.git my_project

## Go to project folder
$ cd my_project

## [Optional] Change port
$ WPDockPort=8081 && sed -i "s/8080:80/$WPDockPort:80/g" docker-compose.yml

## Start containers
$ docker-compose up -d

## Open website in browser
$ browse "http://localhost:$WPDockPort" # Linux
$ open "http://localhost:$WPDockPort" # Mac
```

## WP-CLI from host

You can run WP-CLI commands from the host, just replace `wp` with  `./wpdock` like this:

```sh
# wp cli info
$ ./wpdock cli info
``` 