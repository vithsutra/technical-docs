
# 🌐 Guide to Reverse Proxy

## 🧠 What is a Reverse Proxy?

A **reverse proxy** is a server that sits in front of one or more **backend servers** and forwards **client (user)** requests to the appropriate backend. It **acts on behalf of the server**, hence the name "reverse" proxy.

In simpler terms:
- The client (like a web browser) makes a request.
- That request goes to the reverse proxy.
- The proxy fetches the response from the backend server.
- Then it sends the response back to the client.

> 🔄 Think of it as a **middleman** between the internet and your server that helps control and secure traffic.

---

## 🔁 How Does a Reverse Proxy Work?

```text
Client  --->  Reverse Proxy  --->  Backend Server(s)
         <---                <---
```

### Here's what happens step-by-step:

1. **User sends request** to a website (e.g., `example.com`).
2. The request hits the **reverse proxy**.
3. The proxy decides **which backend** server should handle the request.
4. It forwards the request to that server.
5. The backend server processes it and sends back the response.
6. The proxy receives the response and passes it to the client.

---

## ✨ Why Use a Reverse Proxy? (Features & Benefits)

| Feature | Description |
|--------|-------------|
| 🔒 **SSL Termination** | Handles HTTPS (TLS/SSL) encryption/decryption, reducing load on backend servers. |
| 📈 **Load Balancing** | Distributes traffic across multiple servers to ensure even load and high availability. |
| 🛡️ **Security** | Hides backend IPs, blocks malicious requests, enforces firewall rules. |
| ⚡ **Caching** | Stores frequently requested responses to serve them faster (improves performance). |
| 🔄 **Compression** | Compresses traffic to reduce bandwidth usage and speed up responses. |
| 🌍 **Global Distribution** | Can be used with CDNs (Content Delivery Networks) for geographically distributed users. |
| 🧪 **A/B Testing & Routing** | Routes traffic based on URL, headers, cookies, etc. Useful for testing and blue/green deployments. |

---

## 🔧 Popular Reverse Proxy Servers (Vendors)

Here are some commonly used reverse proxy solutions:

| Tool        | Language      | Performance | Ease of Use | Common Use Cases                      | TLS Support | Load Balancing | Caching | Config Format |
|-------------|---------------|-------------|-------------|----------------------------------------|-------------|----------------|---------|----------------|
| **Nginx**   | C             | ⭐⭐⭐⭐        | ⭐⭐⭐         | Web servers, API gateways              | ✅ Yes       | ✅ Yes          | ✅ Yes  | nginx.conf     |
| **Apache**  | C             | ⭐⭐⭐         | ⭐⭐⭐         | Legacy apps, PHP apps                  | ✅ Yes       | ✅ Yes          | ✅ Yes  | httpd.conf     |
| **HAProxy** | C             | ⭐⭐⭐⭐⭐       | ⭐⭐          | High performance, low latency systems  | ✅ Yes       | ✅ Yes          | ❌ No   | haproxy.cfg    |
| **Traefik** | Go            | ⭐⭐⭐⭐        | ⭐⭐⭐⭐        | Cloud-native, Docker/K8s environments  | ✅ Yes       | ✅ Yes          | ✅ Yes  | YAML/TOML      |
| **Caddy**   | Go            | ⭐⭐⭐⭐        | ⭐⭐⭐⭐⭐       | Simple static sites & HTTPS by default| ✅ Auto HTTPS| ✅ Yes          | ✅ Yes  | Caddyfile      |
| **Envoy**   | C++           | ⭐⭐⭐⭐⭐       | ⭐⭐          | Microservices, Service Mesh (e.g. Istio)| ✅ Yes     | ✅ Yes          | ✅ Yes  | YAML           |

---

## 🔌 Real-World Use Cases

- Running **multiple web apps** on the same VM under different subdomains (`api.example.com`, `app.example.com`).
- **Docker containers** needing exposure to the internet via Traefik.
- **Load balancing** backend APIs in a Kubernetes cluster.
- **Securing** internal services behind a proxy with HTTPS and basic auth.
- Adding a **Web Application Firewall (WAF)** via reverse proxy.

---

## 📦 Reverse Proxy vs Forward Proxy

| Feature           | Forward Proxy                         | Reverse Proxy                         |
|-------------------|----------------------------------------|----------------------------------------|
| Who uses it       | Clients (users)                       | Servers                                |
| Direction         | Client → Proxy → Internet             | Client → Proxy → Server                |
| Use Case          | Accessing blocked content, anonymity  | Load balancing, SSL termination, security |
| Hides             | Client from Server                    | Server from Client                     |

---

## 🚀 Getting Started Example: Nginx as Reverse Proxy

### Install Nginx (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install nginx
```

### Simple Nginx Reverse Proxy Config

Edit `/etc/nginx/sites-available/default`:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Then restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## 🗣️ Contributing

This documentation is part of an open-source effort. If you notice anything missing or incorrect:

- Feel free to **raise an issue**
- Or **submit a pull request** to improve this doc for others!

---

