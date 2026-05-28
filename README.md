# Docker Compose Files

自用 docker compose 配置合集。

## 服务列表

| 目录 | 服务 | 端口 |
|---|---|---|
| anirss | ani-rss + qBittorrent（追番组合） | 7789, 15768 |
| aria | AList | 8244 |
| autobangumi | AutoBangumi | 8892 |
| bilisum | BiliSum（B站视频总结） | 3838 |
| mysql | MySQL 8.0 | 3306 |
| nginx | Nginx 反向代理 | 80, 443 |
| nginxtemp | Nginx 临时实例 | 80, 443 |
| openlist | OpenList（AList 改版） | 5244 |
| peerBanHelper | BT Peer 封禁 | 9898 |
| projectCompose | 项目专用 Nginx + Prometheus | 80, 9090 |
| prometheus | Prometheus | 9090 |
| qbit | qBittorrent | 8080, 8001 |
| redis | Redis | 6379 |
| uptime-kuma | Uptime Kuma | 8002 |

## Docker 代理配置

如果拉取镜像需要代理，参考 `docker.proxy.conf`：

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo cp docker.proxy.conf /etc/systemd/system/docker.service.d/
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 注意事项

- 所有需要凭据的服务都通过 `.env` 文件配置，不要直接改 docker-compose.yml
- 启动前先编辑对应目录的 `.env` 填入实际值
- `prometheus/` 的卷路径确保与本地一致
