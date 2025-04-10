```markdown
# 🔐 SSH Access Setup with Two SSH Keys on AWS EC2 (Without Key Pair)

This guide walks through the process of launching an EC2 instance on AWS **without choosing an SSH key pair**, and then **adding two custom SSH keys** to enable secure access.

---

## 🚀 Step 1: Launch EC2 Instance (Without SSH Key Pair)

1. Go to the [AWS EC2 Console](https://console.aws.amazon.com/ec2).
2. Click on **"Launch Instance"**.
3. Choose:
   - Amazon Linux 2 / Ubuntu (any OS of your choice)
   - Instance type: t2.micro (Free tier)
4. In the **Key Pair (login)** section, select:
   ```
   Proceed without a key pair (Not recommended)
   ```
5. Configure networking and firewall (enable port 22 for SSH).
6. Launch the instance.

---

## 🖥️ Step 2: Connect via EC2 Instance Connect

Since no key pair was selected, use **EC2 Instance Connect**:

1. In the EC2 dashboard, select your instance.
2. Click **Connect** > **EC2 Instance Connect (browser-based SSH)**.
3. Click **Connect**.

---

## 🔑 Step 3: Generate Two SSH Key Pairs Locally

On your local machine:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/key1
ssh-keygen -t rsa -b 4096 -f ~/.ssh/key2
```

This creates:

- `~/.ssh/key1` and `~/.ssh/key1.pub`
- `~/.ssh/key2` and `~/.ssh/key2.pub`

---

## 📂 Step 4: Add Both Public Keys to EC2 Instance

Inside the instance (via EC2 Instance Connect):

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste contents of both `key1.pub` and `key2.pub`, one per line.

Example:

```
ssh-rsa AAAAB3... key1
ssh-rsa AAAAB3... key2
```

Then set permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## 🔗 Step 5: SSH Into EC2 Using Either Key

Now, from your local machine:

```bash
ssh -i ~/.ssh/key1 ubuntu@<EC2-PUBLIC-IP>
# or
ssh -i ~/.ssh/key2 ubuntu@<EC2-PUBLIC-IP>
```

---

## 🎯 Stretch Goal: Install Fail2Ban

To prevent brute-force SSH attacks:

```bash
sudo apt update && sudo apt install fail2ban -y
```

Basic config will protect your SSH automatically.

---

## ✅ Final Outcome

You can now SSH into your server with **either of the two keys** you created — even though no key pair was selected during EC2 creation.

---

### 💡 Author

Created with ❤️ by [Sachin Dayal](https://github.com/Sachin-960)
```

---

This is a part of [roadmap.sh](https://roadmap.sh/projects/ssh-remote-server-setup) Devops Projects.
