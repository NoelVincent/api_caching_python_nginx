# api_caching_python_nginx

```sh
# docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                           NAMES
4a6fb54d0c96   fujikomalan/ipstack:v1   "python3 app.py"         4 minutes ago   Up 4 minutes                                                   ipstack2
df7b455a6c9b   redis:latest             "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   6379/tcp                                        redis
83011d56bfe1   fujikomalan/ipstack:v1   "python3 app.py"         4 minutes ago   Up 4 minutes                                                   ipstack1
a0d8af6eab48   fujikomalan/ipstack:v1   "python3 app.py"         4 minutes ago   Up 4 minutes                                                   ipstack3
3efa770cae5f   nginx:alpine             "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   LB
```

```sh
/root/redis/
├── cert
│   ├── api_cache.key
│   └── api_cache.pem
├── conf
│   └── nginx.conf
└── docker-compose.yml
```

Using a self signed certificate
https://www.sslshopper.com/ssl-checker.html#hostname=redcache.noelvincent.tech

https://redcache.noelvincent.tech/ip

# Dockerizing - API caching - python application with nginx Loadbalancing
Here, it is designed with an nginx proxy loadbalancer and a Redis cache container for api caching. All the communications between the containers are through a bridge network inside the host server.

Using ipstack API in this python application as a demo. This API will show you the Geolocation of IP addresses.

# Architecture
![](https://i.ibb.co/VYp8s1H/1.jpg)
