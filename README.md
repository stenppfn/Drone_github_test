# drone-demo
test


# 配置 Drone `Docker Compose`
```yaml
version: '3'
services:
  drone-server:
    image: drone/drone:1
    ports:
      - 443:443
      - 80:80
    volumes:
      - drone-data:/data:rw
      - ./ssl:/etc/certs
    restart: always
    environment:
      - DRONE_AGENTS_ENABLED=true
      - DRONE_SERVER_HOST=${DRONE_SERVER_HOST:-https://drone.yeasy.com}
      - DRONE_SERVER_PROTO=${DRONE_SERVER_PROTO:-https}
      - DRONE_RPC_SECRET=${DRONE_RPC_SECRET:-secret}
      - DRONE_GITHUB_SERVER=https://github.com
      - DRONE_GITHUB_CLIENT_ID=${DRONE_GITHUB_CLIENT_ID}
      - DRONE_GITHUB_CLIENT_SECRET=${DRONE_GITHUB_CLIENT_SECRET}
  drone-agent:
    image: drone/drone-runner-docker:1
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:rw
    environment:
      - DRONE_RPC_PROTO=http
      - DRONE_RPC_HOST=drone-server
      - DRONE_RPC_SECRET=${DRONE_RPC_SECRET:-secret}
      - DRONE_RUNNER_NAME=${HOSTNAME:-demo}
      - DRONE_RUNNER_CAPACITY=2
    dns: 114.114.114.114
volumes:
  drone-data:
```


### 新建 .env 文件，输入变量及其值
```shell
# 必填 服务器地址，例如 drone.domain.com
DRONE_SERVER_HOST=
DRONE_SERVER_PROTO=https
DRONE_RPC_SECRET=secret
HOSTNAME=demo
# 必填 在 GitHub 应用页面查看
DRONE_GITHUB_CLIENT_ID=
# 必填 在 GitHub 应用页面查看
DRONE_GITHUB_CLIENT_SECRET=
```

# 启动 Drone
````shell
docker-compose up -d
```













