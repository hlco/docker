### 工程目录
```
docker
├── nginx # NGINX 服务
│   ├── conf.d # 配置
│   │   └── default.conf # 默认配置
│   ├── logs # 日志
│   ├── ssl # 证书
│   ├── Dockerfile # docker 镜像文件
│   └── nginx.conf # 配置文件
├── php # PHP 服务
│   ├── conf.d 配置
│   ├── Dockerfile # docker 镜像文件
│   └── php.ini # 配置文件
├── redis # REDIS 服务
│   ├── data # 数据
│   └── redis.conf # 配置文件
├── mysql # 服务日志
│   ├── data # 数据
│   └── logs # 日志
└── wwwroot # 静态资源
```