# Qwen2API

Qwen2API 是一个基于 Node.js 的 API 服务，提供聊天模型的接口以及图像上传功能。

## 目录结构

```

Qwen2API/
├── Dockerfile
├── docker-compose.yml
├── .env
├── package.json
└── src/
    ├── index.js
    └── image.js

```

## 配置文件

### .env 文件

```plaintext
    API_PREFIX=
    LISTEN_ADDRESS=0.0.0.0
    SERVICE_PORT=3000
    API_KEY=sk-123456
    ACCOUNT_TOKENS=
```

- API_PREFIX: 服务路径
    > API 路径，不填则为空(http://localhost:3000)

    > 示例(/api) 则访问 http://localhost:3000/api
- LISTEN_ADDRESS: 监听地址
    > 监听地址，不填则为0.0.0.0

- SERVICE_PORT: 服务运行端口
    > 如果需要修改Docker暴露端口，请修改ports中的参数

    >示例(8080:3000) 则访问 http://localhost:8080
- API_KEY: 密钥
    > API 密钥 (非必填)

    > 如果需要使用多账户或使用内置账户，请填写此项
- ACCOUNT_TOKENS: 账号token
    > 账号token：多个账号使用","分隔

    > 示例：ey1...,ey2...,ey3...

## 安装与运行

### 先决条件

确保您已安装以下软件：

- [Node.js](https://nodejs.org/) (LTS 版本)
- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)（可选）

### 使用 Docker 运行

1. 使用 Docker 命令：

   ```bash
   docker run -d -p 3000:3000 -e LISTEN_ADDRESS=0.0.0.0 -e API_KEY=sk-123456 -e ACCOUNT_TOKENS=ey1...,ey2...,ey3... -e API_PREFIX= -e SERVICE_PORT=3000 --name qwen2api rfym21/qwen2api:latest
   ```

2. 使用 docker-compose 运行服务：

   ```bash
   curl -o docker-compose.yml https://raw.githubusercontent.com/Rfym21/Qwen2API/refs/heads/main/docker-compose.yml
   docker compose pull && docker compose up -d
   ```

### 本地运行

2. 下载仓库文件：

   ```bash
   git clone https://github.com/Rfym21/Qwen2API
   cd Qwen2API
   ```

2. 安装依赖：

   ```bash
   npm install
   ```

3. 创建 `.env` 文件并设置变量：

   ```plaintext
   API_PREFIX=
   LISTEN_ADDRESS=0.0.0.0
   SERVICE_PORT=3000
   API_KEY=sk-123456
   ACCOUNT_TOKENS=ey1...,ey2...,ey3...
   ```

4. 启动服务：

   ```bash
   npm start
   ```

### 复制 Hugging Face 空间

- [Qwen2API](https://huggingface.co/spaces/devme/q2waepnilm)

## API 端点

### 获取授权令牌

- 登录 [QwenLM](https://chat.qwenlm.ai) 打开开发者调试工具获取

![aa0350d1a79c4bdf2ac55b9a374b4b777cb2b512_2_1380x760.png](https://s2.loli.net/2025/02/21/syXqpR3V5OAcDol.png)

### 获取模型列表

- **请求方式**: `GET`
- **URL**: `/v1/models`
- **Headers**:
  - `Authorization`: 必须提供有效的授权令牌。

### 聊天完成

- **请求方式**: `POST`
- **URL**: `/v1/chat/completions`
- **Headers**:
  - `Authorization`: 必须提供有效的授权令牌。
- **请求体**:

  ```json
  {
    "model": "模型名称",
    "messages": [
      {
        "role": "user",
        "content": "用户消息"
      }
    ],
    "stream": false
  }
  ```

## 获取授权令牌

- 登录 [QwenLM](https://chat.qwenlm.ai) 打开开发者调试工具获取

![get_token.png](https://s2.loli.net/2025/02/21/syXqpR3V5OAcDol.png)

### 上传图像

在发送聊天消息时，如果消息包含图像，API 会自动处理图像上传。

## 依赖

- `axios`: 用于发送 HTTP 请求。
- `body-parser`: 解析请求体。
- `dotenv`: 加载环境变量。
- `express`: Web 框架。
- `form-data`: 处理表单数据。
- `uuid`: 生成唯一标识符。
