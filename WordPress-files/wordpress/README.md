# WordPress 博客技术栈

这是一个基于 **WordPress**、**Caddy** (自动化 HTTPS)、**MariaDB** 和 **Redis** 构建的轻量级、安全且注重隐私的博客系统。专为信息安全专业的你设计。

## 目录

1. [前提条件](#前提条件)
2. [快速开始](#快速开始)
3. [配置说明](#配置说明)
4. [安全检查](#安全检查)
5. [维护管理](#维护管理)

## 前提条件

- 已安装 **Docker** 和 **Docker Compose**。
- 拥有一个 **域名** (例如 `blog.example.com`) 并解析到服务器的公网 IP。
- 防火墙放行 `80` 和 `443` 端口。

## 快速开始

1. **配置环境变量**:
    复制示例配置文件并进行编辑。

    ```bash
    cp .env.example .env
    nano .env
    ```

    - 将 `DOMAIN_NAME` 修改为你的实际域名。
    - 将 `DB_PASSWORD` 和 `DB_ROOT_PASSWORD` 修改为强密码。

2. **启动服务**:

    ```bash
    docker compose up -d
    ```

3. **完成安装**:
    访问 `https://your-domain.com` 完成 WordPress 的初始化设置。

## 本地 / 虚拟机运行指南 (无域名)

如果你没有服务器和域名，仅在本地虚拟机 (如 VMware) 体验，请使用此模式。

1. **启动本地模式**:

    ```bash
    # 使用专门的 local 配置文件
    docker compose -f compose.local.yml up -d
    ```

2. **访问**:
    打开浏览器访问虚拟机 IP (例如 `http://192.168.x.x`) 或 `http://localhost`。
    注意：此模式仅开启 HTTP (端口 80)，浏览器可能会提示不安全，属正常现象。

## 配置说明

### 架构概览 (Architecture)

- **Caddy**: 作为入口网关 (反向代理)。负责自动申请和更新 Let's Encrypt 安全证书，管理 HTTPS。
- **WordPress (FPM)**: 后端 PHP 容器。运行在隔离网络中，不直接对外暴露端口。
- **MariaDB**: 数据库存储。
- **Redis**: 内存对象缓存，用于加速页面加载。

### 核心文件

- `docker-compose.yml`: 定义服务编排和网络。
- `Caddyfile`: Web 服务器配置及安全响应头设置。
- `.env`: 存储敏感信息和环境变量。

## 安全检查

应该定期检查博客系统安全状态。

### 1. 服务健康检查 (Health Check)

确保所有容器状态为 `healthy`:

```bash
docker compose ps
```

### 2. 验证 HTTPS 和 安全头

检查是否启用了 HSTS、X-Frame-Options 等安全机制:

```bash
curl -I https://your-domain.com
```

预期输出应包含:

- `Strict-Transport-Security`
- `X-Frame-Options: SAMEORIGIN`

### 3. 漏洞扫描

使用专业工具对自己的博客进行自查 (仅限授权测试):

```bash
# 使用临时的 WPScan 容器进行扫描
docker run -it --rm wpscanteam/wpscan --url https://your-domain.com
```

## 维护管理

### 数据备份

**数据库备份**:

```bash
docker exec blog_db mysqldump -u wordpress_user -p[你的密码] wordpress > backup_db.sql
```

**文件备份**:
备份 `wp_data` 卷中的内容，推荐使用 Restic 或直接打包 Docker 卷。

### 系统更新

拉取最新镜像并重启服务:

```bash
docker compose pull
docker compose up -d
```

## 作者与版权

- **作者**: Crystal & evalEvil
- **版权**: @2026 重庆渝信网安科技服务有限公司
