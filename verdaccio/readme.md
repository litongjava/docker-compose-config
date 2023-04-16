##### 安装docker-compose
```
apt install docker-compose
```
##### 创建配置文件
首先，创建 /home/verdaccio 目录，以下在该目录下操作。
其次，创建 conf 目录，并添加 verdaccio 的 config.yaml 配置文件。

```
mkdir -p ~/verdaccio/conf
# 启动docker镜像,名称verdaccio， 端口 4873
# 拷贝出 verdaccio 镜像的配置文件。
# docker cp：在容器和本地文件系统之间，拷贝文件或文件夹。
# 删除docker镜像
docker rm -f verdaccio
docker run -it --name verdaccio -p 4873:4873 verdaccio/verdaccio
```

```
docker cp verdaccio:/verdaccio/conf/config.yaml ~/verdaccio/conf
docker rm -f verdaccio
```

##### 使用docker-compose启动
接着处理映射目录，
```
mkdir -p ~/code/docker-compose-config/verdaccio/conf
cd ~/code/docker-compose-config/verdaccio/conf
````
添加 docker-compose.yml 文件，使用 docker-compose up 命令启动
```
version: '3'
services:
  verdaccio:
    image: verdaccio/verdaccio
    user: root
    container_name: "verdaccio"
    network_mode: "bridge"
    environment:
      - VERDACCIO_PORT=4873
    ports:
      - "4873:4873"
    volumes:
      - "~/verdaccio/storage:/verdaccio/storage"
      - "~/verdaccio/conf:/verdaccio/conf"
      - "~/verdaccio/plugins:/verdaccio/plugins"
    network_mode: "bridge"
```
前台启动
```
docker-compose up
```
后台启动
```
docker-compose up -d
```
测试是否启动成功
```
# cd vue工程
npm set registry http://localhost:4873/
npm install
```
查看本地目录文件
```
tree /home/verdaccio/*
```
