### 工程目录
```
docker
├── build
│   ├── php72
│   │   └── Dockerfile
│   ├── nginx
│   │   └── Dockerfile
│   ├── mysql
│   │   └── Dockerfile
│   ├── redis
│   │   └── Dockerfile
│   └── docker-compose.yml
├── config
│   ├── redis
│   │   └── redis.conf
│   ├── mysql
│   │   ├── conf.d
│   │   └── my.cnf
│   ├── nginx
│   │   ├── conf.d
│   │   ├── ssl
│   │   └── nginx.conf
│   ├── php
│   │   ├── conf.d
│   │   └── php72.ini
├── data
│   ├── mysql
│   └── redis
├── logs
│   ├── mysql
│   ├── nginx
│   └── php
└── www
```