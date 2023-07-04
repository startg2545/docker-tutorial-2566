## MongoDB on Docker Hub

https://hub.docker.com/_/mongo

### Run a MariaDB container

```bash
$ docker run -dp 27017:27017 --name my-mongodb --env MONGO_INITDB_ROOT_USERNAME=root --env MONGO_INITDB_ROOT_PASSWORD=example mongo:latest
```

### Adding docker-compose.yml

```yaml
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
```

### Download MongoDB client

- https://studio3t.com/download-studio3t-free/
- https://www.mongodb.com/try/download/compass
