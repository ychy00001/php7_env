# 启动
```
docker-cmpose up -d
```

# 项目配置
## 拉取项目
将项目拉取至www目录下，同时配置nginx的配置文件指向项目目录，默认已经有了Laputa的Nginx配置

## 进入容器安装其他软件更新依赖
```
docker-compose exec php-fpm bash
cd www/Laputa
composer install --ignore-platform-reqs
./vendor/bin/thrift_gen.sh
bower install -V --allow-root
```

## 更改env的redis配置需要更改
```
############# database.conf  miss ################

REDIS_DEFAULT_HOST=redis
REDIS_DEFAULT_PASSWORD=


CACHE_HOST=redis
CACHE_PASSWORD=


REDIS_SESSION_HOST=redis
REDIS_SESSION_PASSWORD=

REDIS_QUEUE_HOST=redis
REDIS_QUEUE_PASSWORD=


REDIS_WECHAT_TOKEN_HOST=redis
REDIS_WECHAT_TOKEN_PASSWORD=


CACHE_ARM_HOST=redis
CACHE_ARM_PASSWORD=



INFOER_REDIS_HOST=redis
INFOER_REDIS_PASSWORD=


REDIS_ARM_HOST=redis
REDIS_ARM_PASSWORD=



CRM_REDIS_HOST=redis
CRM_REDIS_PASSWORD=


SSO_REDIS_HOST=redis
SSO_REDIS_DB=

SCM_QUEUE_REDIS_HOST=redis
SCM_QUEUE_REDIS_PASSWORD=



SCM_CACHE_REDIS_HOST=redis
SCM_CACHE_REDIS_PASSWORD=
```


# 目录介绍
data: 数据存储/备份目录
log: 各应用日志目录
phpdocker: 
 - phpdocker/nginx
  - phpdocker/nginx/alpine  nginx alpine镜像封装
  - phpdocker/nginx/conf.d   nginx 配置文件
 - phpdocker/php-fpm  php-fpm的docker镜像封装及配置
www: 应用目录


# 备注
```
# update node
RUN apt-get -y --no-install-recommends install openssl

RUN ln -s /usr/bin/nodejs /usr/bin/node \
    && npm install npm@latest -g \
    && npm install -g n \
    && n stable \
    && rm -rf /usr/bin/node \
    && ln -s /usr/local/bin/node /usr/bin/node
```
