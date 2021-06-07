# drupal-with-docker
how to get up and running with Drupal on a Docker image


How to Docker Drupal


1. Set up mysql db
    
    ➜  ~ docker run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=password mysql:latest

    verify :
    
    ➜  ~ docker ps
	
   | CONTAINER ID | IMAGE | COMMAND | CREATED | STATUS | PORTS | NAMES |
   | --- | --- | --- | --- | --- | --- | --- |
   | f70a31c5eb02 | mysql:latest | "docker-entrypoint.s…" | 44 seconds ago | Up 43 seconds | 3306/tcp, 33060/tcp | mysqldb |

2. create a container for Drupal

    ➜  ~ docker run -d --name drupal --link mysqldb -p 8080:80 -e MYSQL_USER:root -e MYSQL_PASSWORD:password drupal 
	
    ➜  ~ docker ps

    | CONTAINER ID | IMAGE | COMMAND | CREATED | STATUS | PORTS | NAMES |
    | --- | --- | --- | --- | --- | --- | --- |
    |38b87f3b04d9   |drupal        | "docker-php-entrypoi…"   |About a minute ago  | Up About a minute |  0.0.0.0:8080->80/tcp  | drupal|
    |f70a31c5eb02   |mysql:latest  | "docker-entrypoint.s…"  | 5 minutes ago       | Up 5 minutes       | 3306/tcp, 33060/tcp  |  mysqldb|

	http://localhost:8080
	returns http://localhost:8080/core/install.php 
	
3. Set the advanced options 
	database name: drupal
	database username: root
	database password: password
	host: mysqldb
	port 3306

4. now you are up and running with Drupal on Docker!
