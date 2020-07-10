### 工程目录
```
docker
├── build # 镜像
│   ├── php72
│   │   └── Dockerfile
│   ├── nginx
│   │   └── Dockerfile
│   ├── mysql
│   │   └── Dockerfile
│   ├── redis
│   │   └── Dockerfile
│   └── docker-compose.yml
├── config # 服务器配置
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
├── data # 服务数据
│   ├── mysql
│   └── redis
├── logs # 服务日志
│   ├── mysql
│   ├── nginx
│   └── php
└── www # 静态资源
```