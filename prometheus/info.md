### 1. **创建配置文件目录**
```bash
mkdir -p /prometheus
```

---

### 2. **创建配置文件 `prometheus.yml`**
```bash
cd /prometheus
cat <<EOF > prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
EOF
```

---

### 3. **使用 Docker Compose 部署**
创建 `docker-compose.yml`：
```bash
vi docker-compose.yml
```

写入内容：
```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: always
    ports:
      - "9090:9090"
    volumes:
      - /prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.enable-lifecycle'

volumes:
  prometheus_data:
```

---

### 4. **启动服务**
```bash
docker-compose up -d
```

---

### 5. **验证安装**
- **检查容器状态**：
  ```bash
  docker ps | grep prometheus
  ```
- **访问 Web UI**：
  `http://<宿主机IP>:9090`

---

### 6. **热重载配置文件（可选）**
修改配置文件后无需重启容器：
```bash
curl -X POST http://localhost:9090/-/reload
```

---

### 附加说明
1. **监控其他服务**：在 `prometheus.yml` 中添加新的 `scrape_configs`，例如：
   ```yaml
   - job_name: "node-exporter"
     static_configs:
       - targets: ["node-exporter:9100"]
   ```

2. **持久化存储**：使用 Docker 卷 `prometheus_data` 确保数据安全，可通过 `docker volume inspect prometheus_data` 查看卷路径。

3. **与 Grafana 集成**：
   - 安装 Grafana 容器并将其连接到 Prometheus。

