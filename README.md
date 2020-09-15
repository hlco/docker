### 工程目录
```
docker
├── nginx # nginx 服务
│   ├── conf.d # 服务配置
│   │   ├── blog.conf 
│   │   └── default.conf # 默认配置
│   ├── ssl # 证书
│   ├── logs # 日志
│   ├── Dockerfile # docker 文件
│   └── nginx.conf # 配置
├── PHP # PHP 服务
│   ├── conf.d # 服务配置
│   │   └── mysql.conf
│   └── php.ini # 配置
├── redis # redis 服务
│   ├── data # redis 数据
│   └── redis.conf # redis 配置
├── mysql # mysql 服务
│   ├── data # 数据
│   ├── logs # 日志
│   └── my.cnf # 配置
└── www # 静态资源
```