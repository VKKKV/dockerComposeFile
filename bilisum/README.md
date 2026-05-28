# BiliSum Docker 部署

## 直接 `docker compose up -d` 为什么失败？

错误信息：
```
pull access denied for lycohana/bilisum, repository does not exist
or may require 'docker login': denied: requested access to the resource is denied
```

原因：Docker Hub 上不存在 `lycohana/bilisum` 镜像。项目 README 里虽然写了 `docker pull lycohana/bilisum:latest`，但这个镜像从未被发布过。作者只发了 GitHub Release 和 npm 包，没有 push Docker 镜像。

## 解决方案

本地构建镜像：

```bash
git clone https://github.com/lycohana/BiliSum.git
cd BiliSum
npm install --prefix apps/desktop
npm run docker:build
```

这会构建一个名为 `bilisum:local` 的镜像，然后在 `docker-compose.yml` 中使用 `image: bilisum:local` 替换原本的 `lycohana/bilisum:latest`。

## 目录结构

```
bilisum/
├── README.md           # 本文件
├── .env                # 环境变量（API Key 等）
├── .gitignore          # 忽略 .env
└── docker-compose.yml  # docker compose 配置
```

## 使用方式

1. 编辑 `.env`，填入你的 API Key：

```
VIDEO_SUM_ACCESS_TOKEN=你的访问密钥
VIDEO_SUM_LLM_ENABLED=true
VIDEO_SUM_LLM_BASE_URL=https://coding.dashscope.aliyuncs.com/v1
VIDEO_SUM_LLM_MODEL=qwen3.5-plus
VIDEO_SUM_LLM_API_KEY=你的LLM-API-Key
VIDEO_SUM_TRANSCRIPTION_PROVIDER=multimodal
```

2. 启动：

```bash
cd /home/kita/code/dockerComposeFile/bilisum
docker compose up -d
```

3. 访问 `http://127.0.0.1:3838`

## 环境变量说明

| 变量 | 说明 | 必填 |
|---|---|---|
| `VIDEO_SUM_ACCESS_TOKEN` | 访问密钥，设一个长随机串 | 是 |
| `VIDEO_SUM_LLM_ENABLED` | 是否启用 LLM 总结 | 是 |
| `VIDEO_SUM_LLM_BASE_URL` | LLM API 地址 | 是 |
| `VIDEO_SUM_LLM_MODEL` | LLM 模型名 | 是 |
| `VIDEO_SUM_LLM_API_KEY` | LLM API Key | 是 |
| `VIDEO_SUM_TRANSCRIPTION_PROVIDER` | ASR 转写方式，可选 `siliconflow` / `multimodal` | 否，默认 siliconflow |

如果不用 SiliconFlow ASR，设 `VIDEO_SUM_TRANSCRIPTION_PROVIDER=multimodal`，转写走 LLM 接口，不需要额外 API Key。
