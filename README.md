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
