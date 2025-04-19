
# 📘 Caddy vs Nginx: A Detailed Comparison

## 🔍 Overview

| Feature                | **Caddy**                                           | **Nginx**                                     |
|------------------------|----------------------------------------------------|-----------------------------------------------|
| SSL (HTTPS)           | ✅ Automatic via Let's Encrypt                      | ❌ Manual (or use Certbot + config)           |
| HTTP to HTTPS         | ✅ Auto redirect                                    | ❌ Manual config required                     |
| Configuration Style   | 🧾 Simple, Human-readable (Caddyfile)              | 🧱 Declarative, but more verbose               |
| Reverse Proxy         | ✅ Built-in                                         | ✅ Built-in                                    |
| Load Balancing        | ✅ Built-in (round-robin, etc.)                    | ✅ Built-in (with config)                      |
| Static File Server    | ✅ Built-in                                         | ✅ Built-in                                    |
| Performance           | ⚡ Good (Go-based, modern)                          | ⚡ Great (very mature)                         |
| Extensibility         | 🧩 Plugins, APIs                                    | 🧩 Lua, modules, 3rd-party tools               |
| Docker Compatibility  | ✅ Official image with HTTPS                       | ✅ Official image, but manual HTTPS setup      |
| System Resource Usage | 🔋 Low                                              | 🔋 Lower (written in C)                        |
| Dynamic Config Reload | ✅ Hot reload with zero downtime                    | ✅ Via `nginx -s reload`                       |
| TLS DNS Challenge     | ✅ Built-in (for wildcard certs)                    | ❌ Needs extra tools                           |

---

## 🧠 When to Choose What?

### ✅ Use **Caddy** When:
- You want **automatic HTTPS** without touching certbot.
- You're in a **startup/small team** and need speed and simplicity.
- You have a **frequently changing IP** or **dynamic domain setup**.
- You want quick setup for local dev, internal tools, IoT, etc.

### ✅ Use **Nginx** When:
- You need **extremely high performance** under massive scale.
- You require **fine-grained control** (advanced load balancing, caching, etc.).
- You're integrating with **legacy systems or large enterprise** stacks.
- Your org already has **deep experience with Nginx**.

---

# 📘 Caddy Web Server: Complete Guide (Basics to Advanced)

---

## 📌 Section 1: Getting Started with Caddy

### 🔧 Installation

#### On Linux (Debian/Ubuntu):
```bash
# 1. Install Necessary Dependencies
sudo apt update
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl gnupg2

# 2. Add Caddy GPG key
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' \
  | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg

# 3. Add the Caddy APT repo securely using the key
echo "deb [signed-by=/usr/share/keyrings/caddy-stable-archive-keyring.gpg] \
https://dl.cloudsmith.io/public/caddy/stable/deb/debian any-version main" \
| sudo tee /etc/apt/sources.list.d/caddy-stable.list

# 4. Update & install
sudo apt update
sudo apt install caddy
```

#### Using Docker:
```yaml
# docker-compose.yml
version: '3.8'
services:
  caddy:
    image: caddy:latest
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config

volumes:
  caddy_data:
  caddy_config:
```

---

## 📌 Section 1.5: Creating and Using a Caddyfile

### 📄 What is a Caddyfile?
The **Caddyfile** is a simple text file that defines how Caddy should serve content or proxy traffic. It’s known for its readability and minimal configuration syntax.

---

### 🛠️ Where to Create the Caddyfile?

#### ✅ For System Installation (Ubuntu/Debian/etc.):
- Default location:  
  ```
  /etc/caddy/Caddyfile
  ```

- To create/edit it:
  ```bash
  sudo nano /etc/caddy/Caddyfile
  ```

- To reload config:
  ```bash
  sudo systemctl reload caddy
  ```

✅ To use a **custom Caddyfile**:
```bash
sudo caddy run --config /path/to/your/Caddyfile --adapter caddyfile
```

---

#### ✅ For Docker:
- Place `Caddyfile` next to your `docker-compose.yml`.
- Ensure it's mounted correctly:
  ```yaml
  volumes:
    - ./Caddyfile:/etc/caddy/Caddyfile
  ```

- Create it with:
  ```bash
  touch Caddyfile
  nano Caddyfile
  ```

- Then run:
  ```bash
  docker-compose up -d
  ```

---

## 📌 Section 2: Caddyfile Basics

### ✅ Minimal Caddyfile Example
```caddyfile
example.com {
    reverse_proxy localhost:8000
}
```
> Automatically enables HTTPS and proxies to localhost:8000

---

## 📌 Section 3: Common Use Cases

### 🔁 Reverse Proxy
```caddyfile
api.example.com {
    reverse_proxy localhost:5000
}
```

### 🌐 Static File Hosting
```caddyfile
files.example.com {
    root * /var/www/files
    file_server
}
```

### 📁 Serve Multiple Sites
```caddyfile
site1.com {
    reverse_proxy localhost:3000
}

site2.com {
    reverse_proxy localhost:4000
}
```

---

## 📌 Section 4: Advanced Features

### 🔐 Basic Authentication
```caddyfile
secure.example.com {
    route {
        basicauth / admin JDJhJDE0JFBtUmJzY3...  # hashed password
        reverse_proxy localhost:8080
    }
}
```

Generate hash:
```bash
caddy hash-password --plaintext 'your-password'
```

---

### 🔃 Load Balancing
```caddyfile
load.example.com {
    reverse_proxy backend1:8000 backend2:8000 backend3:8000
}
```

---

### 🎛️ Environment Variables
```caddyfile
:80 {
    root * ${STATIC_ROOT}
    file_server
}
```

---

### 📡 Wildcard Domains (DNS Challenge)
```caddyfile
*.example.com {
    reverse_proxy localhost:8080
    tls {
        dns cloudflare {env.CLOUDFLARE_API_TOKEN}
    }
}
```

Set token:
```bash
export CLOUDFLARE_API_TOKEN=your_token_here
```

---

### 🧪 Debugging and Logs
```caddyfile
example.com {
    reverse_proxy localhost:8000
    log {
        output file /var/log/caddy/access.log
    }
}
```

---

## 📌 Section 5: Dynamic Config with API

Enable API in global config:
```caddyfile
{
    admin 0.0.0.0:2019
}
```

Example config update:
```bash
curl localhost:2019/config/ -X POST -H "Content-Type: application/json" --data-binary @config.json
```

---

## 📌 Section 6: System Integration

### Auto-start with systemd:
Caddy includes systemd support out of the box:
```bash
sudo systemctl enable caddy
sudo systemctl start caddy
```

To reload:
```bash
sudo systemctl reload caddy
```

---

## 📌 Section 7: Tips & Best Practices

- ✅ Always use **domain names**, not IPs.
- ✅ Prefer **wildcard certs + DNS challenge** for many subdomains.
- ✅ Mount Docker volumes for `/data` and `/config` to persist HTTPS certs.
- ✅ Keep your `Caddyfile` in **version control**.
- ✅ Use **hot reload API** for dynamic environments.
- ✅ Use multiple Caddy containers behind a load balancer for high availability.


