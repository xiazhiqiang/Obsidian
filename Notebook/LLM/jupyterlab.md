
## 安装初始化

```sh
docker pull jupyter/minimal-notebook

docker run -d -p 9888:8888 -e JUPYTER_TOKEN=jupyternotebook -e JUPYTER_PASSWORD=jupyternotebook -e JUPYTER_ENABLE_LAB=yes jupyter/minimal-notebook 

# 结果（password自动生成hash）：9d50cfd7a9e215a52b81eeb159cd49865f0fa0dd6cfdd8fd4aeb9f0fd62ff544
```

本地访问：http://localhost:9888，输入token即可进入Notebook页面。

## 界面配置中文

```sh
docker exec -it <container_id> bash

pip --version
pip install jupyterlab-language-pack-zh-CN
```

浏览器刷新：http://localhost:9888，在菜单栏里面Settings-》Language中出现选择中文

## 启动时设置自定义样式

- 写好样式文件，例如：~/work/src/llm/jupyter/custom/custom.css，记得给custom.css权限赋值为0777
- ~~启动时挂载目录（映射到内部的路径/home/jovyan/.jupyter/custom）：未生效
```sh
docker run -d -p 9888:8888 -e JUPYTER_TOKEN=jupyternotebook -e JUPYTER_PASSWORD=jupyternotebook -e NOTEBOOK_ARGS="--LabApp.custom_css=True" -d --restart=always -v ~/work/src/llm/jupyter/custom:/home/jovyan/.jupyter/custom jupyter/minimal-notebook

# 结果：8a824ffb4a483b385c5880d3c8ad6655db53f94a64398b65a86cc4420bda3393
```

- 创建容器，但不启动容器中的服务
```sh
docker run -itd -p 9888:8888 -e JUPYTER_TOKEN=jupyternotebook -e JUPYTER_PASSWORD=jupyternotebook --entrypoint=bash --restart=always reg.docker.alibaba-inc.com/sfm/jupyter:minimal
```
- 进入容器中，创建~/.jupyter/custom目录，copy 本地custom.css到custom目录。
- 执行启动服务命令
```sh
start-notebook.py --ServerApp.port 8888 --ServerApp.token='jupyternotebook'  --LabApp.custom_css=True
```
