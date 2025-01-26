在 Ubuntu 上使用 Nginx 作为 Web 服务器非常简单。以下是安装和使用 Nginx 的完整步骤：

### 1. **安装 Nginx**

首先，你需要在 Ubuntu 上安装 Nginx。可以通过 `apt` 包管理器安装 Nginx。

```bash
sudo apt update
sudo apt install nginx
```

### 2. **启动 Nginx 服务**

安装完成后，你可以启动 Nginx 服务。使用以下命令启动 Nginx 服务：

```bash
sudo systemctl start nginx
```

如果你希望 Nginx 在系统启动时自动启动，可以使用以下命令：

```bash
sudo systemctl enable nginx
```

### 3. **检查 Nginx 是否正常运行**

你可以检查 Nginx 服务的状态，确保它已正确启动：

```bash
sudo systemctl status nginx
```

输出应该显示 Nginx 正在运行。例如：

```bash
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2025-01-16 13:35:25 UTC; 10min ago
       Docs: man:nginx(8)
   Main PID: 12345 (nginx)
      Tasks: 3 (limit: 1152)
     Memory: 2.4M
        CPU: 24ms
     CGroup: /system.slice/nginx.service
             ├─12345 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             ├─12346 nginx: worker process
             └─12347 nginx: worker process
```

### 4. **访问 Nginx 默认页面**

Nginx 安装并启动后，默认情况下它会将网页根目录设置为 `/var/www/html`。你可以在浏览器中访问服务器的 IP 地址，确认 Nginx 是否正在运行。

打开浏览器，访问：

```
http://your_server_ip
```

如果你看到一个 Nginx 欢迎页面，表示 Nginx 已成功安装和启动。

### 5. **配置防火墙（可选）**

如果你使用 UFW（Uncomplicated Firewall）作为防火墙，可以确保防火墙允许 HTTP 和 HTTPS 流量通过。使用以下命令允许流量通过 80（HTTP）和 443（HTTPS）端口：

```bash
sudo ufw allow 'Nginx Full'
```

然后可以检查防火墙状态：

```bash
sudo ufw status
```

### 6. **修改默认配置**

Nginx 的默认配置文件通常位于 `/etc/nginx/sites-available/default`。你可以编辑该文件，配置你的网站。

```bash
sudo nano /etc/nginx/sites-available/default
```

以下是一个基本的配置示例：

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

这个配置文件指定了网站的根目录（`/var/www/html`）和默认的欢迎页面。

### 7. **重启 Nginx**

修改配置文件后，需要重新加载 Nginx 配置。你可以使用以下命令来重启 Nginx：

```bash
sudo systemctl restart nginx
```

### 8. **配置 Nginx 反向代理（可选）**

如果你希望使用 Nginx 作为反向代理服务器，可以将请求转发到后端应用（如 Node.js 或其他 Web 应用）。以下是一个简单的反向代理配置示例：

```nginx
server {
    listen 80;

    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;  # 将请求转发到后端应用（例如 Node.js 在 3000 端口）
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

在此配置中，所有请求都会被转发到本地的 3000 端口，假设后端应用正在该端口运行。

### 9. **配置 SSL（可选）**

你可以为 Nginx 配置 SSL 以启用 HTTPS。可以使用 Let’s Encrypt 提供的免费 SSL 证书：

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com
```

按照提示完成证书申请和配置，Certbot 会自动为你配置 SSL。

### 10. **检查 Nginx 错误日志**

如果遇到问题，检查 Nginx 的错误日志非常有用。Nginx 的错误日志通常位于 `/var/log/nginx/error.log`。

```bash
sudo tail -f /var/log/nginx/error.log
```

### 总结：

1. 安装 Nginx：`sudo apt install nginx`
2. 启动 Nginx：`sudo systemctl start nginx`
3. 访问默认页面：`http://your_server_ip`
4. 配置防火墙：`sudo ufw allow 'Nginx Full'`
5. 修改 Nginx 配置文件：`/etc/nginx/sites-available/default`
6. 配置反向代理或启用 HTTPS（可选）

以上是 Ubuntu 上使用 Nginx 的基本步骤。如果你遇到任何问题，欢迎继续提问！