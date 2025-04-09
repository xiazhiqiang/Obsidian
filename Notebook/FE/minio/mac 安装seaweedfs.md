
```sh
docker pull chrislusf/seaweedfs
```

## 快捷启动

```sh
# docker run -p 8333:8333 chrislusf/seaweedfs server -s3
docker run -p 9333:9333 -p 8080:8080 -p 8333:8333 chrislusf/seaweedfs server -s3

# 查找容器名
docker ps | grep seaweedfs

docker exec -it [容器名] weed shell
# 或
docker exec -it [容器名] /bin/sh
weed shell

# 配置
s3.configure -access_key=seaweedadmin -secret_key=seaweedadmin -user=admin -actions=Read,Write,List,Tagging,Admin -buckets=zwzt-oss -apply
# 或
s3.configure \
  -access_key=seaweedadmin \
  -secret_key=seaweedadmin \
  -buckets=zwzt-oss \
  -user=admin \
  -actions=Read,Write,List,Tagging,Admin
  -apply
```
执行后结果
```json
{

  "identities":  [

    {

      "name":  "admin",

      "credentials":  [

        {

          "accessKey":  "seaweedadmin",

          "secretKey":  "seaweedadmin"

        }

      ],

      "actions":  [

        "Read:zwzt-oss",

        "Write:zwzt-oss",

        "List:zwzt-oss",

        "Tagging:zwzt-oss",

        "Admin:zwzt-oss"

      ],

      "account":  null

    }

  ],

  "accounts":  []

}
```
## compose启动

seaweedfs-compose-dev.yml
```yml
version: '3.9'

services:

  master:

    image: chrislusf/seaweedfs # use a remote image

    ports:

      - 9333:9333

      - 19333:19333

    command: "master -ip=master -ip.bind=0.0.0.0"

  volume:

    image: chrislusf/seaweedfs # use a remote image

    ports:

      - 8080:8080

      - 18080:18080

    command: 'volume -mserver="master:9333" -port=8080 -ip=volume -ip.bind=0.0.0.0'

    depends_on:

      - master

  filer:

    image: chrislusf/seaweedfs # use a remote image

    ports:

      - 8888:8888

      - 18888:18888

    command: 'filer -master="master:9333" -ip.bind=0.0.0.0'

    depends_on:

      - master

      - volume

  s3:

    image: chrislusf/seaweedfs # use a remote image

    ports:

      - 8333:8333

    command: 's3 -filer="filer:8888" -ip.bind=0.0.0.0'

    depends_on:

      - master

      - volume

      - filer

  webdav:

    image: chrislusf/seaweedfs # use a remote image

    ports:

      - 7333:7333
    command: 'webdav -filer="filer:8888"'

    depends_on:

      - master

      - volume

      - filer
```

```yml
version: '3.9'

services:

master:

image: chrislusf/seaweedfs # use a remote image

ports:

- 9333:9333

- 19333:19333

- 9324:9324

command: "master -ip=master -ip.bind=0.0.0.0 -metricsPort=9324"

volume:

image: chrislusf/seaweedfs # use a remote image

ports:

- 8080:8080

- 18080:18080

- 9325:9325

command: 'volume -mserver="master:9333" -ip.bind=0.0.0.0 -port=8080 -metricsPort=9325'

depends_on:

- master

filer:

image: chrislusf/seaweedfs # use a remote image

ports:

- 8888:8888

- 18888:18888

- 9326:9326

command: 'filer -master="master:9333" -ip.bind=0.0.0.0 -metricsPort=9326'

tty: true

stdin_open: true

depends_on:

- master

- volume

s3:

image: chrislusf/seaweedfs # use a remote image

ports:

- 8333:8333

- 9327:9327

command: 's3 -filer="filer:8888" -ip.bind=0.0.0.0 -metricsPort=9327'

depends_on:

- master

- volume

- filer

webdav:

image: chrislusf/seaweedfs # use a remote image

ports:

- 7333:7333

command: 'webdav -filer="filer:8888"'

depends_on:

- master

- volume

- filer

```

```sh
docker compose -f seaweedfs-compose.yml -p seaweedfs up

docker ps | grep seaweedfs

# 进入master容器，compose架构需要进入master容器操作
docker exec -it [master容器名] weed shell

# 设置bucket
s3.bucket.create -filter=filter:8888 -name=zwzt-oss

# 访问验证http://127.0.0.1:8333 或 http://127.0.0.1:8888
```
## 参考

https://www.godhearing.cn/seaweedfs-xiang-jie-bu-shu-sheng-chan/
https://github.com/seaweedfs/seaweedfs/blob/master/docker/README.md
https://github.com/seaweedfs/seaweedfs/wiki/Amazon-S3-API
https://github.com/seaweedfs/seaweedfs/wiki/Client-Libraries

