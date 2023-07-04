## MariaDB on Docker Hub

https://hub.docker.com/_/mariadb

### Run a MariaDB container

```bash
$ docker run -dp 3306:3306 --name my-mariadb --env MARIADB_USER=demo-user --env MARIADB_PASSWORD=demo-password --env MARIADB_ROOT_PASSWORD=my-root-pw  mariadb:latest
```

### Adding docker-compose.yml

```yaml
version: "3.1"
services:
  db:
    image: mariadb
    restart: always
    ports:
      - "33066:3306"
    environment:
      MARIADB_USER: demo-user
      MARIADB_PASSWORD: demo-password
      MARIADB_ROOT_PASSWORD: my-root-pw
```
