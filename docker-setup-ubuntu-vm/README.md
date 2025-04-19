# 🚀 Docker Engine Setup on Ubuntu VM

## ✅ Supported Ubuntu Versions

Docker officially supports the following 64-bit Ubuntu versions:
- Ubuntu 24.04 (LTS)
- Ubuntu 22.04 (LTS)
- Ubuntu 20.04 (LTS)

---

## 🧹 Step 1: Uninstall Conflicting Packages

Run this to remove any existing Docker-related packages:

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
  sudo apt-get remove -y $pkg
done
```

---

## 📦 Step 2: Set Up Docker APT Repository

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

---

## 🐳 Step 3: Install Docker Engine

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## ✅ Step 4: Verify Installation

```bash
sudo docker run hello-world
```

You should see a message: **"Hello from Docker!"**

---

## 👤 (Optional) Allow Docker Commands Without `sudo`

```bash
sudo usermod -aG docker $USER
```

> Log out and log back in for the changes to take effect.

---

## 🔄 To Uninstall Docker Completely

```bash
sudo apt-get purge -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker /var/lib/containerd
sudo rm /etc/apt/sources.list.d/docker.list
sudo rm /etc/apt/keyrings/docker.asc
```

---

That's it! 🎉 Your Ubuntu VM is now Docker-ready.

Let me know if you'd like this as a PDF or markdown file.
