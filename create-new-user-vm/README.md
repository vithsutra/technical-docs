# 👤 Create a New User on Linux VM

Use this guide to quickly create a new user on your Ubuntu VM and set up the essential environment.

---

## 🛠️ Step-by-Step Instructions

### 1. Create the user with a home directory and bash shell

```bash
sudo useradd -m -s /bin/bash <username>
```

- `-m`: Creates the home directory (e.g., `/home/username`)
- `-s /bin/bash`: Sets bash as the default shell

📌 Replace `<username>` with the desired username.

---

### 2. Set a password for the user

```bash
sudo passwd <username>
```

Enter the new password when prompted.

---

### 3. (Optional) Give the user **sudo access**

```bash
sudo usermod -aG sudo <username>
```

This adds the user to the `sudo` group so they can run commands as root when needed.

---

### 4. (Optional) Switch to the new user

```bash
su - <username>
```

---
