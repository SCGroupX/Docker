# Docker

将 go 开发的 web 应用容器化。

### 1. 安装

网上安装教程很多，由于我们使用的CentOS系统，具体参照[教程](http://www.runoob.com/docker/centos-docker-install.html)

### 2.Dockerfile

编写前后端Dockerfile如下

前端：

```
FROM registry.cn-shenzhen.aliyuncs.com/nodedev/nodejs:10.12.0-alpine

ADD . /app/
WORKDIR /app

RUN npm install
RUN npm rebuild node-sass --force

ENV HOST 0.0.0.0

ENV PORT 8000

EXPOSE 8000
CMD ["npm","run","dev"]
```

后端：

```
FROM golang:latest

LABEL name="iog320"

WORKDIR $GOPATH/src/github.com/SCGroupX/SWAPI/service

ADD . $GOPATH/

RUN go build .

EXPOSE 8080

ENTRYPOINT  ["./service"]
```

### 3. 创建Docker Image

```
docker build -t swapi .
```

> 注意不要忘记最后的点

查看docker image

```
docker images
```

### 4.部署应用

编写 stack`.yml` 文件如下：

```yml
version: '3'

service:
  server:
    build: ./service
    links: 
      - db
    ports:
      - 8080:8080
    environment:
      DB_HOST: db:3306
      DB_PASSWORD: root

client:
  build: ./client
  links: 
    - service
  ports:
    - 8000:8000
```

### 5.启动

```
docker-compose -f stack.yml up -d swapi
```

