单机部署多服务，使用 nginx 端口映射会比较方便。nginx 配置的文档网上一堆，不再赘述，此处只谈如何用 docker 来部署。

# 前提

使用 Docker 部署 nginx 之前，需要先了解一个概念：Docker 的文件映射（匿名卷）。因为 Docker 需要保持容器存储层不发生写操作，保证容器存储层的无状态化。

nginx 的日志和 项目 文件都是需要频繁变化的，所以需要将这部分映射到宿主机上。

## VOLUME

命令格式为：

* `VOLUME ["<路径1>", "<路径2>"...]`

这是定义匿名卷的，主要是在构建镜像的时候使用这个是为了防止用户在运行时没有将动态文件所保存的目录挂载为卷。

在运行时可以覆盖在构建的时候指定的挂载设置：

`docker run -d -v mydata:/data xxxx`

这个命令中是使用 `mydata` 这个命名卷挂载到了 `/data` 这个位置，代替了构建中定义的匿名卷的挂载配置。

# 用 Docker 部署 nginx

`docker run --name emrys-nginx -p 80:80 -d -v /opt/nginx/html:/usr/share/nginx/html:ro -v /opt/nginx/conf.d:/etc/nginx/conf.d:ro -v /opt/nginx/logs:/var/log/nginx -v /opt/nginx/config/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx`

值得注意的是，在配置 nginx 的时候，路径要注意是容器的路径，而不是宿主机的路径，我在这里 404 了半天