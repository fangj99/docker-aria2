# docker-aria2

[![](https://images.microbadger.com/badges/version/opengg/aria2.svg)](https://microbadger.com/images/opengg/aria2 "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/opengg/aria2.svg)](https://microbadger.com/images/opengg/aria2 "Get your own image badge on microbadger.com")

[![Docker Pulls](https://img.shields.io/docker/pulls/opengg/aria2.svg)](https://hub.docker.com/r/opengg/aria2/ "Docker Pulls")
[![Docker Stars](https://img.shields.io/docker/stars/opengg/aria2.svg)](https://hub.docker.com/r/opengg/aria2/ "Docker Stars")
[![Docker Automated](https://img.shields.io/docker/automated/opengg/aria2.svg)](https://hub.docker.com/r/opengg/aria2/ "Docker Automated")

## Usage

0. Prepare config and download directories with following commands.

    ```bash
    mkdir /storage/aria2/config
    chown -R 1001:1002 /storage/aria2/config
    find /storage/aria2/config -type d -exec chmod 755 {} +
    find /storage/aria2/config -type f -exec chmod 644 {} +
    mkdir /storage/aria2/downloads
    chown -R 1001:1002 /storage/aria2/downloads
    find /storage/aria2/downloads -type d -exec chmod 755 {} +
    find /storage/aria2/downloads -type f -exec chmod 644 {} +
    ```
0. Put `aria2.conf` file in the config dir, with following content.

    ```ini
    save-session=/config/aria2.session
    input-file=/config/aria2.session
    save-session-interval=60

    dir=/downloads

    file-allocation=prealloc
    disk-cache=128M

    enable-rpc=true
    rpc-listen-port=6800
    rpc-allow-origin-all=true
    rpc-listen-all=true

    rpc-secret=<password>

    auto-file-renaming=false

    max-connection-per-server=16
    min-split-size=1M
    split=16
    ```
0. Run following command to start aria2 instance

    ```bash
    docker run \
      -d \
      --name aria2 \
      -u=1001:1002 \
      -v /storage/aria2/config:/config \
      -v /storage/aria2/downloads:/downloads \
      -p 6800:6800 \
      opengg/aria2
    ```

Note:
* Make sure the download folder is writable by the given uid/gid.
* `aria2.conf` will be created automatically if not exists.
* Learn more about tuning `aria2.conf`: [Config reference](https://aria2.github.io/manual/en/html/aria2c.html#aria2-conf)

## Parameters

The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side.
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
`http://192.168.x.x:8080` would show you what's running INSIDE the container on port 80.


* `-p 6800:6800` - the port(s)
* `-v /storage/aria2/config:/config` - where aria2 should store config files and logs
* `-v /storage/aria2/downloads:/downloads` - local path for downloads
* `-u 1001` for UserID - see below for explanation
* `-u 1001:1002` for GroupID - see below for explanation

It is based on alpine linux, for shell access whilst the container is running do `docker exec -it aria2 /bin/sh`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `-u 1001:1002`. To find yours use `id user` as below:

```bash
  $ id dockeruser
    uid=1001(dockeruser) gid=1002(dockergroup) groups=1002(dockergroup)
```









### 下载神器aria2

```
$ sudo add-apt-repository ppa:t-tujikawa/ppa
$ sudo apt-get update
$ sudo apt-get install aria2
```





* [aria2](https://aria2.github.io/)
* [Mac下载神器aria2  - 简书](http://www.jianshu.com/p/1290f8e7b326)
* [aria2 WebUI](http://ziahamza.github.io/webui-aria2/)
* [Aria2 & YAAW 使用说明](http://aria2c.com/usage.html) **☆**
* [Mac下使用Aria2下载教程----迅雷和百度盘终极解决方案(可突破百度盘限速) - Mac综合讨论区 -  威锋论坛 - 威锋网](http://bbs.feng.com/read-htm-tid-9585996.html)
* [Aria2配置教程（Mac和Windows） – Medium](https://medium.com/@Justin___Smith/aria2%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-mac%E5%92%8Cwindows-b31d0f64bd4e#.sv5yneiie)
* [Mac 上使用百度网盘很烦躁？花点时间配置 aria2 吧 - 少数派](http://sspai.com/32167)
* [使用Aria2下载百度网盘和115的资源](https://blog.icehoney.me/posts/2015-01-31-Aria2-download)
* [如何使用aria2及webui-aria2下载百度云资源 | Jin&#39;s Blog](http://moflying.com/2016/06/05/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8aria2%E5%8F%8Awebui-aria2%E4%B8%8B%E8%BD%BD%E7%99%BE%E5%BA%A6%E4%BA%91%E8%B5%84%E6%BA%90/)
* [GitHub - acgotaku/BaiduExporter: Assistant for Baidu to export download links to aria2/aria2-rpc](https://github.com/acgotaku/BaiduExporter)
* [notes/aria2.md at master · erasin/notes · GitHub](https://github.com/erasin/notes/blob/master/linux/soft/aria2.md)
* [Mac 下使用 Aria2 实现迅雷离线和百度云下载](http://chanjh.com/post/software/0012)
* [Aria2 使用手札（简易部署 + 快速进阶） - RhinoCodec - 博客园](http://www.cnblogs.com/RhinoC/p/aria2.html)
* [把树莓派变成24小时运行的下载神器](https://zhuanlan.zhihu.com/p/21471896)

* [aria2c(1) &mdash; aria2 1.28.0 documentation](https://aria2.github.io/manual/en/html/aria2c.html)


* 使用命令行操作aria2
* [Aria2c 使用举例](http://sydi.org/posts/linux/aria2c-usage-sample-cns.html)  **命令行下的aria2c使用教程**
* [aria2常用命令 - 宣赠超的博客](http://www.xzcblog.com/post-27.html)

***

#### Docker aria2

##### dockerhub

* https://hub.docker.com/r/evlos/aria2/
* [nemoalex/docker-aria2](https://hub.docker.com/r/nemoalex/docker-aria2/)
* https://hub.docker.com/r/vimagick/aria2/
* https://hub.docker.com/r/timonier/aria2/
* https://hub.docker.com/r/binking338/alpine-aria2/


##### Glutton + aria2

* [[aria2 系列文章] 如何使用 Docker 安装最新版的 aria2 - KoolShare改版固件 -  KoolShare -  源于玩家 服务玩家](http://koolshare.cn/thread-41435-1-1.html)
* [nemoalex/docker-aria2](https://hub.docker.com/r/nemoalex/docker-aria2/)
* [GitHub - NemoAlex/docker-compose-aria2: Docker Compose file for https://hub.docker.com/r/nemoalex/docker-aria2](https://github.com/NemoAlex/docker-compose-aria2)
* [使用 Docker 安装 aria2 | glutton cookbook](https://nemoalex.gitbooks.io/glutton-cookbook/content/shi_yong_dockeran_zhuang_aria2.html)  **☆**
* [GitHub - NemoAlex/glutton: A web client for aria2](https://github.com/NemoAlex/glutton)
* [Glutton - 一个新的 aria2 Web 客户端 - V2EX](https://www.v2ex.com/t/270483)



##### Aria2 WebUI + aria2

* [一行命令用 Docker 架设 aria2 服务 - V2EX](https://www.v2ex.com/t/205762)
* [dockerfile/aria2 at master · Evlos/dockerfile · GitHub](https://github.com/Evlos/dockerfile/tree/master/aria2)
* [GitHub - ziahamza/webui-aria2: The aim for this project is to create the worlds best and hottest interface to interact with aria2. Very simple to use, just download and open index.html in any web browser.](https://github.com/ziahamza/webui-aria2)
* [神之下载器——Aria2 - 博客 - binsite](http://www.binss.me/blog/the-wonderful-downloader-aria2/)
* [Docker 架设 aria2 服务 yf &#8211; yf](https://xinkl.cc/wordpress/2016/08/29/docker-%E6%9E%B6%E8%AE%BE-aria2-%E6%9C%8D%E5%8A%A1-yf/) **☆**
* [Aria2 使用手札（简易部署 + 快速进阶） - RhinoCodec - 博客园](http://www.cnblogs.com/RhinoC/p/aria2.html) **☆**
* [GitHub - tianon/gosu: Simple Go-based setuid+setgid+setgroups+exec](https://github.com/tianon/gosu)


***

#### aria2 实现Docker化

1. docker + alpine + aria2
2. docker + alpine + aria2 + Aria2 WebUI

***

#### 可用的aria2配置 aria2.conf

```
#aria2.conf

dir=/aria2down
rpc-listen-port=6800
listen-port=51413
dht-listen-port=51415
rpc-secret=leafney

continue=true

input-file=/etc/aria2/aria2.session
save-session=/etc/aria2/aria2.session
save-session-interval=60
max-concurrent-downloads=3

enable-rpc=true
rpc-allow-origin-all=true
rpc-listen-all=true

bt-enable-lpd=true
follow-torrent=true
enable-dht=true
bt-enable-lpd=true
enable-peer-exchange=true
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
bt-seed-unverified=true
bt-save-metadata=true
```

***

### 关于Docker下配置aria2可参考整理的文件 `Docker下配置aria2.md` 

***

#### VOLUME 下载目录的账户操作权限

通过 gosu 让用户 aria2 来运行 aria2c 程序 
通过 supervisor来管理 aria2c 程序的自启动

其中，通过Docker中的默认root账户来运行 supervisor 时，在 supervisor 的配置文件中要指定运行supervisor的用户账户 `user=root` 。否则，再启动容器后，容器内的 VOLUME 目录的文件操作权限会被改写为 root，导致 aria2账户无法写入文件。

***

#### 设置 BT DHT 文件下载 

默认情况下 aria2 配置好了后，只能进行 http 下载，在下载 BT 或 磁力链接时 的下载速度一直是0 。

需要在配置文件 `aria2.conf` 中 设置 相关的参数 如上即为经测试后可用的配置项，更详细的参数配置请参看：[Aria2 & YAAW 使用说明](http://aria2c.com/usage.html)

***

#### bt种子  磁力链接 DHT 

*.torrent   BT种子文件

magnet:?xt=urn:btih:ED53414D2D48264DE9CC82B68011A1AD91035BE1   磁力链接

通过 magnet 来获取 torrent

***

#### 端口访问 及 nginx 配置

通过查看 aria2的运行日志，我们可以看到：

```
11/16 09:57:15 [^[[1;32mNOTICE^[[0m] IPv4 RPC: listening on TCP port 6800

11/16 09:57:57 [^[[1;32mNOTICE^[[0m] IPv4 DHT: listening on UDP port 51415       
                                                                                                   
11/16 09:57:57 [^[[1;32mNOTICE^[[0m] IPv4 BitTorrent: listening on TCP port 51413 
```

因为 RPC 和 BT 监听的是 TCP 的端口，DHT监听的是 UDP的端口，所以在 nginx配置文件中，就不能加在 `http {}` 配置节点下，需要添加到和 `http {}` 同级的 `stream {}` 下。

详细配置方法见官网文档：[TCP and UDP --nginx](https://www.nginx.com/resources/admin-guide/tcp-load-balancing/)


如果是较低版本的nginx，配置了 `stream{}` 节点后是无法识别的，会报如下错误：

```
2016/11/16 18:46:28 [emerg] 13125#0: unknown directive "stream" in /etc/nginx/nginx.conf:75
``` 

查看我当前服务器中的nginx版本为：

```
$ nginx -v
nginx version: nginx/1.4.6 (Ubuntu)
```

在 nginx的 1.9.0 以上版本才会支持 stream 节点项设置。

在 apt版本库中的nginx版本一般较低，如果想要安装最新的nginx版本，需要添加官方的安装源。

##### Ubuntu 14.04升级nginx到官方最新版本

编辑 `/etc/apt/sources.list` 添加如下两行：

```
$ sudo vim /etc/apt/sources.list

deb http://nginx.org/packages/ubuntu/ trusty nginx
deb-src http://nginx.org/packages/ubuntu/ trusty nginx
```

导入升级key，并更新安装：

```
$ sudo wget http://nginx.org/keys/nginx_signing.key
$ sudo apt-key add nginx_signing.key
$ sudo apt-get update
$ sudo apt-get install nginx
```


升级安装时遇到如下错误信息：

```
Unpacking nginx (1.10.2-1~trusty) over (1.4.6-1ubuntu3.7) ...
dpkg: error processing archive /var/cache/apt/archives/nginx_1.10.2-1~trusty_amd64.deb (--unpack):
 trying to overwrite '/etc/default/nginx', which is also in package nginx-common 1.4.6-1ubuntu3.7
dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/nginx_1.10.2-1~trusty_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

通过如下命令解决：

```
$ sudo apt-get remove nginx-common
$ sudo apt-get install nginx
```

这样就能顺利安装了。

然后启动 nginx，启动时又遇到如下错误：

```
➜  ~ sudo service nginx start                  
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] still could not bind()
```

80端口被占用了。

通过如下命令查看：

```
$ ps -ef|grep nginx
```

发现有四个进程在占用着nginx，然后将这些进程全部杀掉：

```
$ killall -9 nginx

# 重新启动nginx服务
$ sudo service nginx start

$ sudo service nginx status
```

可以看到nginx可以正常启动了。

***

#### Ubuntu下nginx升级到最新版 (第二种操作方法--未测试)

```
sudo apt-add-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get upgrade nginx -y
```

***

#### nginx中 stream节点的设置

stream {} 节点的设置格式参考官网文档：https://nginx.org/en/docs/stream/ngx_stream_core_module.html

配置文件格式如下：

```
upstream rpc {
	server 127.0.0.1:6800;
}

# RPC listening on TCP
server {
	listen 6800;
	proxy_pass rpc;
}

```

不能采用下面的方式：

```
# RPC listening on TCP
server {
	listen 6800;
	proxy_pass http://127.0.0.1:6800;
}

# BitTorrent listening on TCP
server {
	listen 51413;
	proxy_pass http://127.0.0.1:51413;
}
```

会报如下错误：

```
nginx: [emerg] invalid host in upstream "http://127.0.0.1:6800" in /etc/nginx/conf.d/aria2web.stream:4
nginx: configuration file /etc/nginx/nginx.conf test failed
```

也不能采用如下方式：

```
# RPC listening on TCP
server {
	listen 6800;
	proxy_bind 127.0.0.1:6800;
}

# BitTorrent listening on TCP
server {
	listen 51413;
	proxy_bind 127.0.0.1:51413;
}
```

会报如下错误：

```
nginx: [emerg] invalid address "127.0.0.1:6800" in /etc/nginx/conf.d/aria2web.stream:4
nginx: configuration file /etc/nginx/nginx.conf test failed
```

* [Nginx发布1.9.0版本，新增支持TCP代理和负载均衡的stream模块 | 张戈博客](https://zhangge.net/5037.html)
* [nginx  1.9  tcp stream 4](http://danchaofan.blog.51cto.com/1196121/1826732)
* [Mac OS/Linux命令查询网络端口占用情况 - 猫哥_kaiye - 博客园](http://www.cnblogs.com/kaiye/archive/2013/05/25/3099393.html)


***

如果在 docker run 时 指定 了 -p 127.0.0.1:6800:6800 那么 nginx中使用 stream 也绑定了该端口，nginx就无法启动，错误提示为端口已绑定。

***

#### 

在现在可运行的 leafney/alpine-aria2-webui 镜像的基础上，再添加一个 VOLUME 为 `/etc/aria2` 目录，发现容器无法正常启动运行。暂未找到原因。
