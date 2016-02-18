This project contains a Vagrant file for creating a machine with docker and
docker compose installed, and a docker compose file to run mariadb with
a dvol managed volume.

After the first `vagrant up`, vagrant ssh to the box and install dvol via 
the command echoed at the end of provisioning.

Note you can start mariadb via `docker-compose up`. You can also run the mysql
command line against the db via:

<pre>
docker run -it --link vagrant_mariadb_1:mysql --rm mariadb sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
</pre>

Note the first component in the link name must be that echoed by docker-compose.

The `docker-compose down` command will stop maria db
