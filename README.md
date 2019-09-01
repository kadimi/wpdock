WPDock is a very basic but certainly usefull docker setup for working with WordPress. It comes with: 

- PHPMyAdmin (on port [8036](http://localhost:8036))
- the `./wpdock` command to run WP-CLI commands directly from the host

## Create a new WordPress site

```sh
## Prepare settings.
PROJECT_FOLDER=wpdock-project
WPDOCK_PHPMA_PORT=8030
WPDOCK_WP_ADMIN_EMAIL=admin@example.com
WPDOCK_WP_ADMIN_PASSWORD=password
WPDOCK_WP_PORT=8080
WPDOCK_WP_TITLE="WPDock"

##
#
# Do not change below this line.
#
##

WPDOCK_WP_URL="http://localhost:$WPDOCK_WP_PORT"

## Clone repository.
git clone git@github.com:kadimi/wpdock.git $PROJECT_FOLDER

## Go to project folder.
cd $PROJECT_FOLDER

## Change ports
sed -i "s/8080:80/$WPDOCK_WP_PORT:80/g" docker-compose.yml
sed -i "s/8030:80/$WPDOCK_PHPMA_PORT:80/g" docker-compose.yml
sed -i "s/MYSQL_DATABASE: wordpress/MYSQL_DATABASE: wordpress_$PROJECT_FOLDER/g" docker-compose.yml
sed -i "s/WORDPRESS_DB_NAME: wordpress/WORDPRESS_DB_NAME: wordpress_$PROJECT_FOLDER/g" docker-compose.yml

## Start containers.
docker-compose up -d

## Install WordPress
sleep 10
./wpdock core install \
	--admin_user=admin \
	--admin_email=$WPDOCK_WP_ADMIN_EMAIL \
	--admin_password=$WPDOCK_WP_ADMIN_PASSWORD \
	--skip-email \
	--title='$WPDOCK_WP_TITLE' \
	--url=$WPDOCK_WP_URL

## Open website in browser.
if [[ "$OSTYPE" == "linux-gnu" ]]; then
	browse "http://localhost:$WPDOCK_WP_PORT"
elif [[ "$OSTYPE" == "darwin"* ]]; then
	open "http://localhost:$WPDOCK_WP_PORT"
fi
```

## WP-CLI from host

You can run WP-CLI commands from the host, just replace `wp` with  `./wpdock` like this:

```sh
# wp cli info
$ ./wpdock cli info
``` 