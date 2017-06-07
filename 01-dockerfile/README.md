# Dockerfileとdocker buildコマンドでDockerイメージを作る
[http://www.atmarkit.co.jp/ait/articles/1407/08/news031.html](http://www.atmarkit.co.jp/ait/articles/1407/08/news031.html)

```
# Dockerfile
FROM ubuntu
MAINTAINER yoheiMune
RUN apt-get update && \
	apt-get install -y nginx
ADD index.html /var/www/html
```

```
$ docker build -t mune/nginx:1.0 .
Sending build context to Docker daemon 4.096 kB
Step 1/4 : FROM ubuntu
 ---> 0ef2e08ed3fa
Step 2/4 : MAINTAINER yoheiMune
 ---> Using cache
 ---> 6f9f96a5e2c3
Step 3/4 : RUN apt-get update && 	apt-get install -y nginx
 ---> Running in 97cb4d030986
Removing intermediate container 97cb4d030986
Step 4/4 : ADD index.html /usr/share/nginx/html
 ---> 05cd76ba33de
Removing intermediate container cbfa4069ba22
Successfully built 05cd76ba33de
```
```
$ docker images
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
mune/nginx                                           1.0                 05cd76ba33de        2 hours ago         227 MB
```

```
$ docker run --name nginx1 -d -p 8088:80 mune/nginx:1.2 /usr/sbin/nginx -g 'daemon off;' -c /etc/nginx/nginx.conf
709b166330095c38c8537266ae3be803dd0bba9f21f1e53fb614420b1e2f591f
```

```
root@50fe23cf38ba:/# cat /etc/nginx/sites-available/default 
```

```
FROM ubuntu
MAINTAINER yoheiMune
RUN apt-get update && \
	apt-get install -y nginx
ADD index.html /var/www/html
ENTRYPOINT /usr/sbin/nginx -g 'daemon off;' -c /etc/nginx/nginx.conf
```
```
$ docker build -t mune/nginx:1.3 .
Sending build context to Docker daemon 6.144 kB
Step 1/5 : FROM ubuntu
 ---> 0ef2e08ed3fa
Step 2/5 : MAINTAINER yoheiMune
 ---> Using cache
 ---> 6f9f96a5e2c3
Step 3/5 : RUN apt-get update && 	apt-get install -y nginx
 ---> Using cache
 ---> 38c8a6cc3052
Step 4/5 : ADD index.html /var/www/html
 ---> Using cache
 ---> 928f30c2a3f6
Step 5/5 : ENTRYPOINT /usr/sbin/nginx
 ---> Using cache
 ---> 8f5ec8f801c8
Successfully built 8f5ec8f801c8
```

```
$ docker stop nginx1
nginx1
$ docker rm nginx1
nginx1

# または纏めて
$ docker rm -f nginx1
```
```
$ docker run --name nginx1 -d -p 8088:80 mune/nginx:1.4
f9e7d15fd66adc40ee1e7b5c70950b046958f1e645a9266736644425ee680c31

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
f9e7d15fd66a        mune/nginx:1.4      "/bin/sh -c '/usr/..."   2 hours ago         Up 3 seconds        0.0.0.0:8088->80/tcp   nginx1
 
$ curl localhost:8088
Hello from nginx.
```




