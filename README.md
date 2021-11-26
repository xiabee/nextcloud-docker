# Nextcloud-Docker
`nextcloud` + `nginx` + `redis` + `mariadb`一键式搭建个人云盘服务

### 目录结构

```bash
├── docker-compose.yml
└── proxy
    ├── conf.d
    │   └── nextcloud.conf
    └── ssl_certs
        ├── cert.cer
        ├── cert.key
        └── fullchain.cer
```



### 搭建方法

#### 1.修改配置

* 将代码克隆到本地

  ```bash 
  git clone https://github.com/xiabee/nextcloud-docker.git
  ```

* 将域名相关证书放入`ssl_certs`文件夹中

* 修改`/proxy/conf.d/nextcloud.conf`：将`your.domain.com`换成自己的域名

  ```nginx
  server {
      listen 80;
      listen [::]:80;
      server_name your.domain.com;
      # 
      # enforce https
      return 301 https://$server_name:443$request_uri;
  }
  
  server {
      listen 443 ssl http2;
      listen [::]:443 ssl http2;
      server_name your.domain.com;
      ...
  }
  ```

* 设置数据库密码：

  ```yml
  db:
      environment:
            MYSQL_ROOT_PASSWORD: set_your_password
            MYSQL_DATABASE: nextcloud
            MYSQL_USER: set_your_username
            MYSQL_PASSWORD: set_your_pass
  ```

  

#### 2.启动服务

```bash
docker-compose up -d
# 启动容器

docker-compose ps
# 查看容器状态
```

出现以下内容说明运行成功：

```bash
$ docker-compose ps
      Name                     Command               State                       Ports                     
---------------------------------------------------------------------------
nextcloud_app_1     /entrypoint.sh php-fpm           Up      9000/tcp                                      
nextcloud_cache_1   docker-entrypoint.sh redis ...   Up      6379/tcp                                      
nextcloud_db_1      docker-entrypoint.sh --tra ...   Up      3306/tcp                                      
nextcloud_proxy_1   /docker-entrypoint.sh ngin ...   Up      0.0.0.0:5000->443/tcp,:::5000->443/tcp, 80/tcp

```



#### 3. 注册网盘

* 访问你服务器的`5000`端口，进入注册页面即可。

* 运行结果如下：

  ![](https://tva1.sinaimg.cn/large/0084b03xly1gw9u7dpc1qj31hc0smazp.jpg)



#### 附：其他相关命令

```bash
docker-compose restart
# 重启服务
docker-compose stop
# 停止服务
docker-compose down
# 停止并删除服务
```





详细搭建过程与故障排除过程看这里：[https://blog.xiabee.cn/posts/nextcloud-docker/](https://blog.xiabee.cn/posts/nextcloud-docker/)

