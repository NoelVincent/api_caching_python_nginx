# Dockerizing - API caching - python application with nginx Loadbalancing
Here, it is designed with an nginx proxy loadbalancer and a Redis cache container for api caching. All the communications between the containers are through a bridge network inside the host server.

Using ipstack API in this python application as a demo. This API will show you the Geolocation of IP addresses.

# Architecture
![](https://i.ibb.co/VYp8s1H/1.jpg)

### Features
- IP-Location finding website (Python) 
- Easy to migrate everywhere 
- Container load balanced (Nginx)
- All containers are spin up with a single command

### Includes
- 5 Containers (IP-Stack, Redis, Nginx(Load Balancer))
- 1 Network (For Container interconnecting)

## How to get IPstack API
> Please go through the [ipstack](https://ipstack.com/) and click "_GET FREE API KEY_" on the top right corner. 
![alt text](https://i.ibb.co/sFb3zz6/ipstack.png)

# Docker
Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. 

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. You can easily share containers while you work too.
You can learn more about the same in the [website.](https://docs.docker.com/get-started/overview/)

# What Is a Container
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.
You can learn more about the same in the [website.](https://www.docker.com/resources/what-container)


# Docker compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.
You can learn more about the same in the [website.](https://docs.docker.com/compose/)


# Installing Docker
> Here I am using AWS Amazon linux server
```sh
amazon-linux-extras install docker -y
systemctl restart docker.service
systemctl enable docker.service
```
Please see the [website](https://docs.docker.com/engine/install/) for more information.

# Installing Docker compose
```sh
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```
Please see the [website](https://docs.docker.com/compose/install/) for more information.

# File Structure
```sh
/root/redis/
├── cert
│   ├── api_cache.key
│   └── api_cache.pem
├── conf
│   └── nginx.conf
└── docker-compose.yml
```
# Adding SSL certificate
> Using a self signed certificate
- I have my self signed certificate and private key in /root/redis/cert

# Configuration file
```sh
vim nginx.conf
```
```sh
events {
    worker_connections   2000;
}

http {
    
   upstream http_backend{
     server ipstack1:8080;
     server ipstack2:8080;
     server ipstack3:8080;
   }
   
   server {

      listen   443;
      ssl    on;
      ssl_certificate    /etc/ssl/api_cache.pem;
      ssl_certificate_key    /etc/ssl/api_cache.key;

      location / {
          proxy_pass http://http_backend;
          proxy_set_header Host            $host;
          proxy_set_header X-Forwarded-For $remote_addr;
      }
   }
}
```

# Docker Compose
> Note: The compose file should have the name and the extension as docker-compose.yml
```sh
---
version: "3"
services:

  LB:
    image: nginx:alpine
    container_name: LB
    restart: always
    ports:
      - "443:443"
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ./cert/:/etc/ssl/
    networks:
      - api-cache
    
  redis:
    image: redis:latest
    container_name: redis
    restart: always
    networks:
      - api-cache

  ipstack1:
    image: fujikomalan/ipstack:v1
    container_name: ipstack1
    restart: always
    environment:
      - CACHING_SERVER=172.31.45.75
      - IPSTACK_KEY=e895****************************  ### ==> Your ipstack API KEY
    networks:
      - api-cache

  ipstack2:
    image: fujikomalan/ipstack:v1
    container_name: ipstack2
    restart: always
    environment:
      - CACHING_SERVER=172.31.45.75
      - IPSTACK_KEY=e895****************************  ### ==> Your ipstack API KEY
    networks:
      - api-cache

  ipstack3:
    image: fujikomalan/ipstack:v1
    container_name: ipstack3
    restart: always
    environment:
      - CACHING_SERVER=172.31.45.75
      - IPSTACK_KEY=e895****************************  ### ==> Your ipstack API KEY
    networks:
      - api-cache

networks:
  api-cache: 
```
# Execution
- Running a syntax check
```sh
docker-compose config
```
- Executing the yaml file
```sh
docker-compose up -d
```
```sh
# docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                           NAMES
4a6fb54d0c96   fujikomalan/ipstack:v1   "python3 app.py"         4 minutes ago   Up 4 minutes                                                   ipstack2
df7b455a6c9b   redis:latest             "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   6379/tcp                                        redis
83011d56bfe1   fujikomalan/ipstack:v1   "python3 app.py"         4 minutes ago   Up 4 minutes                                                   ipstack1
a0d8af6eab48   fujikomalan/ipstack:v1   "python3 app.py"         4 minutes ago   Up 4 minutes                                                   ipstack3
3efa770cae5f   nginx:alpine             "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   LB
```
## Sample Screenshot
![](https://i.ibb.co/FwZp3pm/site.png)
