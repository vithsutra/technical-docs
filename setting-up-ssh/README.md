
# 🔐 Setting Up SSH Public Key on a Linux VM

This guide helps you:

- ✅ Generate a secure SSH key pair  
- ✅ Set up **key-based SSH login** to a remote Linux VM  
- ✅ Learn both **automatic** and **manual** methods to copy the public key  
- ✅ Understand how to SSH using your **private key**

---

This will now render correctly, with each bullet point on a new line. Let me know if you want a styled emoji-less version too!

---

## 🧠 What is SSH?

**SSH (Secure Shell)** lets you securely access and manage remote machines like servers or VMs.  
You can use a **public-private key pair** instead of typing a password each time. It's safer and more convenient.

---

## ✅ Step-by-Step Guide

### 1️⃣ Generate SSH Key Pair (on your local machine)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**What this does:**
- `-t ed25519`: Creates a modern and strong key type
- `-C`: Adds a label (your email or comment)

Just press `Enter` for the default file path (`~/.ssh/id_ed25519`) when prompted.

📦 Output files:
- `~/.ssh/id_ed25519` → **Private key** (NEVER share this)
- `~/.ssh/id_ed25519.pub` → **Public key** (safe to share)

---

### 2️⃣ Add Your Public Key to the Remote VM

You can choose **either** of the two methods below:

---

#### 🔁 Option 1: Using `ssh-copy-id` (Automatic Method)

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@your-vm-ip
```

- Adds your public key to the VM's `~/.ssh/authorized_keys`
- After this, you can SSH without password

---

#### ✍️ Option 2: Manual Method (Copy and Paste)

1. View your public key on **local machine**:

    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```

2. Copy the full key output.

3. SSH into your VM using password (one last time):

    ```bash
    ssh user@your-vm-ip
    ```

4. On the **VM**, run:

    ```bash
    mkdir -p ~/.ssh
    nano ~/.ssh/authorized_keys
    ```

5. Paste the public key into the file and save (`Ctrl+O`, `Enter`, then `Ctrl+X` in nano).

6. Set correct permissions (on the VM):

    ```bash
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
    ```

✅ Done! Now your VM knows your public key and will trust you when you use your private key to connect.

---

### 3️⃣ SSH into Your VM

You can now SSH directly:

```bash
ssh user@your-vm-ip
```

---

### 🗝️ Use a Private Key Manually with SSH

If you have multiple keys or not using the default, you can **specify the private key** like this:

```bash
ssh -i ~/.ssh/id_ed25519 user@your-vm-ip
```

> `-i`: Tells SSH which private key to use  
> Make sure the key has `600` permissions:  
> `chmod 600 ~/.ssh/id_ed25519`

---

## 📂 Important Files Summary

| File | Location | Purpose |
|------|----------|---------|
| `~/.ssh/id_ed25519` | Local machine | Your **private key** (keep secret) |
| `~/.ssh/id_ed25519.pub` | Local machine | Your **public key** |
| `~/.ssh/authorized_keys` | On VM | Stores allowed public keys |

---

## ⚙️ Basic SSH Usage Recap

| Task | Command |
|------|---------|
| Generate new key pair | `ssh-keygen -t ed25519` |
| Copy public key automatically | `ssh-copy-id -i ~/.ssh/id_ed25519.pub user@vm-ip` |
| Connect using default key | `ssh user@vm-ip` |
| Connect using specific key | `ssh -i ~/.ssh/id_ed25519 user@vm-ip` |

---

## 🙌 That's it!

Now you know how to:
- Create secure SSH keys
- Set up public keys (auto + manual)
- Login using SSH with and without specifying the private key

