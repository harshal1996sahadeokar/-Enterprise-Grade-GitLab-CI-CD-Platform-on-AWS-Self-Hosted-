# 🛠️ Self-Hosted GitLab Setup on AWS (Red Hat + 2GB RAM + 16GB Disk)

## 🧱 Infrastructure Setup (AWS)

- **OS**: Red Hat Enterprise Linux 8 (RHEL 8)
- **Instance Type**: t3.small / t2.small (2 GB RAM)
- **Storage**: 16 GB (gp2/gp3)
- **Security Group**: Open ports 22, 80, 443
- **Elastic IP**: Yes (for static public access)

---

## 🔐 Connect via SSH

```bash
ssh -i your-key.pem ec2-user@<your-public-ip>
---

-------------------------------------------------------------------------
## ⚙️ Install Prerequisites

sudo yum update -y
sudo yum install -y curl policycoreutils openssh-server perl firewalld postfix
sudo systemctl enable firewalld postfix
sudo systemctl start firewalld postfix


🐙 Add GitLab Repository & Install GitLab CE
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="http://<your-public-ip>" yum install -y gitlab-ce

🔁 Configure GitLab
bash
Copy
Edit
sudo gitlab-ctl reconfigure
Access it via: http://<your-ec2-public-ip>

🧠 Optimize for 2GB RAM
Add Swap Memory
bash
Copy
Edit
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
Disable Prometheus to save RAM
bash
Copy
Edit
sudo nano /etc/gitlab/gitlab.rb

# Add:
prometheus_monitoring['enable'] = false
Then:

bash
Copy
Edit
sudo gitlab-ctl reconfigure
🔥 Open Firewall Ports (Optional)
bash
Copy
Edit
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
🧪 Login & First-Time Setup
Visit: http://<your-ec2-public-ip>

Set root password

Login with:

Username: root

Password: your newly set password

📚 Official GitLab References
📘 Install GitLab CE

📘 RHEL / CentOS Install Guide

📘 System Requirements

📘 GitLab Config File (gitlab.rb)

📘 Backup & Restore GitLab

📘 GitLab on AWS
