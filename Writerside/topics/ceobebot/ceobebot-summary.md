# Ceobebot

> 刻俄柏智能的仓库地址为： [github.com/alioth-center/ceobebot-core](https://github.com/alioth-center/ceobebot-core)

此项目正在进行重构，并剥离出 ZeroBotPlugin 的框架中，因为需要提供配置型管理插件，使用硬编码注册/配置/开启的方式管理插件并不适合基于 docker 部署的 ceobebot

## 快速开始

我们建议使用 `docker`/`docker-compose` 的方式来部署 Ceobebot，在此之前，需要先准备三个文件：

1. `config.json`
2. `config.yaml`
3. `public.yaml`

然后使用我们的 `docker-compose.yaml`:

```yaml
version: "3"

services:
    ceobebot-core:
        image: docker.proxy.alioth.center/mlikiowa/napcat-docker:latest
        container_name: ceobebot-core
        environment:
            - ACCOUNT=${YOUR_ACCOUNT}
            - WS_ENABLE=true
        ports:
            - 20001:3001
            - 6099:6099
        volumes:
            - "./configs:/usr/src/app/napcat/config"
            - "./data:/root/.config/QQ"
        network_mode: bridge
        restart: always
    ceobebot-plugin:
        image: ghcr.proxy.alioth.center/sunist-c/zerobot:nightly
        container_name: ceobebot-plugin
        volumes:
            - "./zero/config:/app/config"
            - "./zero/data:/app/data"
```

我们对这一配置做一些说明：

1. 我们使用 `napcat` 作为消息收发的中间件，可以转到 [NapCat](https://github.com/NapNeko/NapCatQQ) 查看更多信息
2.

> `docker.proxy.alioth.center` 是 `registry-1.docker.io` 的代理，你可以直接使用 `mlikiowa/napcat-docker:latest` 这一镜像
>
> 同样，`ghcr.proxy.alioth.center` 是 `ghcr.io` 的代理，你可以直接使用 `ghcr.io/sunist-c/zerobot` 这一镜像

准备工作完成后，运行下面的指令即可启动：

```Shell
docker compose up -d
```