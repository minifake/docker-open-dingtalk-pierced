将[钉钉内网穿透工具](https://developers.dingtalk.com/document/resourcedownload/http-intranet-penetration)封装成容器镜像，以便能够在不同的环境下使用。本镜像最主要是可以支持树莓派等`arm`架构的linux运行.  `Enjoy it`

> 已自动更新容器镜像到 [docker hub](https://hub.docker.com/r/minifake/open-dingtalk-pierced)。

## 使用说明

### 参数说明

| 参数 | 说明 |
| :-- | :-- |
| SUBDOMAIN | 附加到 `vaiwan.com` 的子域名。例如：SUBDOMAIN=abcdef888，在服务启动成功后，可通过 `abcdef888.vaiwan.com` 来访问被代理（穿透）的本地服务。|
| PORT | 被代理（穿透）的本地服务端口，例如：`8080`、`172.17.0.1:8081` 等。 |

注意事项：
- 由于主域名 `vaiwan.com` 唯一，因此，你所设置的 SUBDOMAIN 有可能已被他人占用了，建议：尽量使用特殊一点的 SUBDOMAIN 值（例如：`公司名+业务模块+环境`等格式的值）。
- 将工具封装成 docker 容器镜像后启动，从容器中访问宿主机 IP 时，MacOS、Linux 有所不同，注意确定网络的网关地址。

### 通过 docker命令行 启动
```bash
sudo docker run -itd --name=c_open-dingtalk-pierced \
    -p 4040:4040 \
    -e SUBDOMAIN=abcdef888 \
    -e PORT=172.17.0.1:8086 \
    minifake/open-dingtalk-pierced
```

### 使用 docker-compose 启动

```bash
cp docker-compose.yml.example docker-compose.yml

# vim docker-compose.yml

sudo docker-compose up -d
```

查看容器状态：

```bash
sudo docker ps -f 'name=c_open-dingtalk-pierced'

CONTAINER ID        IMAGE                          COMMAND             CREATED             STATUS              PORTS                    NAMES
2a90e9372323        minifake/open-dingtalk-pierced   "/run.sh"           4 minutes ago       Up 41 seconds       0.0.0.0:4040->4040/tcp   c_open-dingtalk-pierced
```

查看容器日志：

```bash
sudo docker logs -f `docker ps -q -f 'name=c_open-dingtalk-pierced'`
```

访问 `http://localhost:4040`，进入界面管理。
