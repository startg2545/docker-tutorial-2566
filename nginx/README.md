## Running Nginx instance

```
$ docker run -dp 8080:80 --name my-nginx nginx:alpine
```

## Running Nginx instance with volume mount

```
$ docker run -dp 8081:80 --name my-nginx2 -v ./html:/usr/share/nginx/html nginx:alpine
```