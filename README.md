[![](https://images.microbadger.com/badges/image/jgeusebroek/spotweb.svg)](https://microbadger.com/images/jgeusebroek/spotweb "Get your own image badge on microbadger.com")
# Docker Spotweb image

An image running [ubuntu/16.10](https://github.com/ubuntu/16.10) Linux and [Spotweb](https://github.com/spotweb/spotweb).

## Requirements

You need a seperate MySQL / MariaDB server. This can of course be a (linked) docker container but also a dedicated database server.


## Usage

	docker run --restart=always -d -p 80:80 \
		--hostname=spotweb \
		--name=spotweb \
		-v <hostdir_where_config_will_persistently_be_stored>:/config \
		-e TZ='Europe/Amsterdam'
		-e SPOTWEB_DB_TYPE=pdo_mysql \
		-e SPOTWEB_DB_HOST=<database_server_hostname> \
		-e SPOTWEB_DB_NAME=spotweb \
		-e SPOTWEB_DB_USER=spotweb \
		-e SPOTWEB_DB_PASS=spotweb \
		jgeusebroek/spotweb

You should now be able to reach the spotweb interface on port 80, and you can configure Spotweb. The install URL: 'http://yourhost/install.php'

### Automatic retreiving of new spots

To enable automatic retreiving, you need to setup a cronjob on the docker host.

	*/15 * * * * docker exec spotweb /usr/bin/php /var/www/spotweb/retrieve.php >/dev/null 2>&1

This example will retrieve new spots every 15 minutes.

### Updates

The container will try to auto-update the database when a newer version image is  released.


### Environment variables

* `TZ` The timezone the server is running in. Defaults to `Europe/Amsterdam`.
* `SPOTWEB_DB_TYPE` Database type. Use `pdo_mysql` for MySQL.
* `SPOTWEB_DB_HOST` The hostname / IP of the database server.
* `SPOTWEB_DB_NAME` The database used for spotweb.
* `SPOTWEB_DB_USER` The database server username.
* `SPOTWEB_DB_PASS` The database server password.

## License

MIT / BSD

## Author Information

[Jeroen Geusebroek](https://jeroengeusebroek.nl/)
