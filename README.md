# frp内网穿透全端口映射方案（支持ssh、http、https）

# 仓库简介
[![码云Gitee](https://gitee.com/bkywksj/frp/badge/star.svg?theme=blue)](https://gitee.com/bkywksj/frp)
[![GitHub](https://img.shields.io/github/stars/bkywksj/frp.svg?style=social&label=Stars)](https://github.com/bkywksj/frp)
使用docker-compose部署内网全端口映射 支持ssh http https


### 仓库目录结构 https://gitee.com/bkywksj/frp.git
- frp_0.51.3_windows_amd64 windows下的frps（服务端）和frpc（客户端）
- frpc.ini docker-compose脚本frpc.yml启动客户端的时候调用的配置文件
- frpc.yml docker-compose脚本启动客户端
- frps.ini docker-compose脚本frps.yml启动服务端的时候调用的配置文件
- frps.yml docker-compsoe脚本启动服务端
- nginx.conf nginx配置文件
- nginx.yml docker-compose脚本启动nginx

## 部署步骤

### 部署nginx（如服务器有则无需使用脚本启动，直接增加核心配置即可）
复制nginx.conf到/docker/nginx/conf目录下（不要修改或修改为nginx.yml对应配置的挂载目录下）
复制nginx.yml到/docker/nginx目录下
```bash
cd /docker/nginx
$ docker-compose -f nginx.yml up -d nginx
```

### 部署frp-server
修改frps.ini配置适配自己的域名后，复制frps.ini和frps.yml到/home/frps目录下（或自定义目录）
```bash
$ cd /home/frps
$ docker-compose -f frps.yml up -d frps
```

### linux部署frp-client
修改frpc.ini配置适配自己的域名后，复制frpc.ini和frpc.yml到/home/frpc目录下（或自定义目录）,
如果是windows则使用frp_0.51.3_windows_amd64下的文件进行配置运行
```bash
$ cd /home/frpc
$ docker-compose -f frpc.yml up -d frpc
```

### windows部署frp-client
复制frp_0.51.3_windows_amd64到自己想存放的目录下
打开frpc.ini进行配置（可以创建快捷方式到桌面）
启动run.bat（可以创建快捷方式到桌面）



---------------------------------- docker和docker-compose安装 --------------------------------------------------


### docker和docker-compose安装
安装相关工具类

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```

配置docker仓库

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

aliyun的源

```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

```

安装docker

```bash
yum install docker-ce
```
启动docker

```bash
systemctl start docker
```

设置开机自启

```bash
chkconfig docker on
```

## **配置163镜像与存储目录**

```bash
vim /etc/docker/daemon.json
```

registry-mirrors 为镜像地址
docker 版本<20 graph 为存储目录 建议不要使用默认的 否则空间会不够用
docker 版本>20 graph已经弃用 需使用 data-root

```json
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"],
  "data-root": "/home/docker"
}
```

```bash
systemctl daemon-reload
systemctl restart docker
```


# docker-compose安装

#### 手动安装

下载地址：https://github.com/docker/compose/releases/
服务器无法访问GitHub，所以先下载对应的版本到本地，并改名为 docker-compose，然后上传到服务器的/usr/local/bin目录

####  授权
```bash
chmod +x /usr/local/bin/docker-compose
```

