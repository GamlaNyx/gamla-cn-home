# 部署指南 — gamla.cn

## 服务器信息

| 项目 | 值 |
|------|-----|
| 域名 | gamla.cn |
| 公网 IP | 39.106.105.106 |
| Web Server | Nginx |
| 代码仓库 | https://github.com/GamlaNyx/gamla-cn-home.git |

## 1. 服务器初始化

SSH 登录服务器后，安装必要工具：

```bash
ssh root@39.106.105.106

# 安装 Node.js 22.x (LTS)
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
apt-get install -y nodejs

# 安装 Nginx
apt-get install -y nginx

# 安装 Git
apt-get install -y git

# 验证
node -v   # v22.x
npm -v    # 10.x
nginx -v  # 1.x
```

## 2. 拉取并构建项目

```bash
# 创建站点目录
mkdir -p /var/www/gamla.cn
cd /var/www/gamla.cn

# 拉取代码
git clone https://github.com/GamlaNyx/gamla-cn-home.git .

# 安装依赖 + 构建
npm install
npm run build

# 构建产物在 dist/ 目录
ls dist/
# 输出：index.html  blogs/  demos/  photos/  gamla.ico
```

## 3. 配置 Nginx

创建站点配置：

```bash
vim /etc/nginx/sites-available/gamla.cn
```

写入以下配置：

```nginx
server {
    listen 80;
    server_name gamla.cn www.gamla.cn;

    root /var/www/gamla.cn/dist;
    index index.html;

    # Gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml text/javascript;
    gzip_min_length 1024;

    # 静态资源缓存
    location ~* \.(ico|svg|jpg|png|webp|css|js)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    # SPA / 干净 URL 路由
    location / {
        try_files $uri $uri/ $uri.html =404;
    }

    # 安全头
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
}
```

启用站点：

```bash
# 创建软链接
ln -s /etc/nginx/sites-available/gamla.cn /etc/nginx/sites-enabled/

# 测试配置
nginx -t

# 重载 Nginx
systemctl reload nginx
```

## 4. 配置 DNS

在域名管理后台（阿里云/Cloudflare 等）添加 DNS 记录：

| 类型 | 主机记录 | 记录值 |
|------|---------|--------|
| A | @ | 39.106.105.106 |
| A | www | 39.106.105.106 |

## 5. HTTPS（可选，推荐）

使用 Let's Encrypt 免费证书：

```bash
# 安装 certbot
apt-get install -y certbot python3-certbot-nginx

# 自动配置证书 + 更新 Nginx 配置
certbot --nginx -d gamla.cn -d www.gamla.cn

# 测试自动续期（证书 90 天过期，certbot 会自动续）
certbot renew --dry-run
```

## 6. 后续更新

每次更新代码后执行：

```bash
cd /var/www/gamla.cn
git pull
npm install        # 如果依赖有变化
npm run build      # 重新构建
# Nginx 直接读取 dist/ 目录，无需重载
```

## 7. 验证

浏览器访问：
- http://gamla.cn（HTTP）
- https://gamla.cn（配置 HTTPS 后）
- http://39.106.105.106（IP 直接访问）
