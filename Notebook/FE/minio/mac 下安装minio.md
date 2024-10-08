## Docker安装minio

```sh
docker pull minio/minio
```
## 启动

```sh
docker run -p 9000:9000 -p 9090:9090 \
    --name minio \
    -d --restart=always \
    -e "MINIO_ACCESS_KEY=minioadmin" \
    -e "MINIO_SECRET_KEY=minioadmin" \
    -v /Users/chengmu/Downloads/temp/minio/data:/data \
    minio/minio server \
    /data --console-address ":9090" -address ":9000"
```

- 9090 端口指的是 minio 的客户端端口。虽然设置 9090，但是我们在访问 9000 的时候，他也会自动跳到 9090。
- 9000 端口是 minio 的服务端端口，我们程序在连接 minio 的时候，就是通过这个端口来连接的。
- -v 就是 docker run 当中的挂载，这里的/mydata/minio/data:/data 意思就是将容器的/data 目录和宿主机的/mydata/minio/data 目录做映射，这样我们想要查看容器的文件的时候，就不需要看容器当中的文件了。
    - 注意在执行命令的时候，他是会自动在宿主机当中创建目录的。我们不需要手动创建。
    - minio 所上传的文件默认都是存储在容器的 data 目录下的！
    - 假如删除容器了宿主机当中挂载的目录是不会删除的。假如没有使用-v 挂载目录，那他在宿主机的存储位置的文件会直接删除的。
    - 宿主机的挂载目录一定是根目录，如果是相对路径会有问题。还有容器当中的目录也是必须是绝对路径（根路径就是带/的）。
    - 所谓的挂载其实就是将容器目录和宿主机目录进行绑定了，操作宿主机目录，容器目录也会变化，操作容器目录，宿主机目录也会变化。这样做的目的 可以间接理解为就是数据持久化，防止容器误删，导致数据丢失的情况。
- MINIO_ACCESS_KEY:账号 MINIO_SECRET_KEY:密码 (正常账号应该不低于 3 位，密码不低于 8 位，不然容器会启动不成功)
- --console-address 指定客户端端口
- -d --restart=always 代表重启 linux 的时候容器自动启动
- --name minio 容器名称

## 查看服务状态

```
docker ps 
```

## 访问
[http://localhost:9000](http://localhost:9000/) 默认密码：minioadmin/minioadmin
