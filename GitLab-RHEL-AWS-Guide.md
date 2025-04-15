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

## ⚙️ Install Prerequisites
