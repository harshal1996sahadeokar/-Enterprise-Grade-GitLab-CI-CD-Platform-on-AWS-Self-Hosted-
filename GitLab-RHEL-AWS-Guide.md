# 🛠️ Self-Hosted GitLab Setup on AWS (Red Hat + 2GB RAM + 16GB Disk)

To set up a self-hosted GitLab instance on AWS using Red Hat Enterprise Linux 8 (RHEL 8), follow these steps:

1. **Infrastructure Setup (AWS)**:
   - **OS**: Red Hat Enterprise Linux 8 (RHEL 8)
   - **Instance Type**: t3.small / t2.small (2 GB RAM)
   - **Storage**: 16 GB (gp2/gp3)
   - **Security Group**: Open ports 22, 80, 443
   - **Elastic IP**: Yes (for static public access)

2. **Connect via SSH**:
   - Use the following command to connect to your EC2 instance:
   
     ```bash
     ssh -i your-key.pem ec2-user@<your-public-ip>
     ```

3. **Install Prerequisites**:
   - Update your system and install the necessary packages:

     ```bash
     sudo yum update -y
     sudo yum install -y curl policycoreutils openssh-server perl firewalld postfix
     sudo systemctl enable firewalld postfix
     sudo systemctl start firewalld postfix
     ```

4. **Add GitLab Repository & Install GitLab CE**:
   - Add the GitLab repository and install GitLab Community Edition (CE):

     ```bash
     curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
     sudo EXTERNAL_URL="http://<your-public-ip>" yum install -y gitlab-ce
     ```

5. **Configure GitLab**:
   - Run the following command to configure GitLab:

     ```bash
     sudo gitlab-ctl reconfigure
     ```

   - Once configured, you can access GitLab at [http://<your-ec2-public-ip>](http://<your-ec2-public-ip>).

6. **Optimize for 2GB RAM**:
   - To optimize performance, add swap memory and disable Prometheus monitoring:

     **Add Swap Memory**:
     ```bash
     sudo fallocate -l 2G /swapfile
     sudo chmod 600 /swapfile
     sudo mkswap /swapfile
     sudo swapon /swapfile
     echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
     ```

     **Disable Prometheus to Save RAM**:
     - Edit the GitLab configuration file to disable Prometheus:

       ```bash
       sudo nano /etc/gitlab/gitlab.rb
       ```

     - Add the following line:

       ```bash
       prometheus_monitoring['enable'] = false
       ```

     - Then reconfigure GitLab:

       ```bash
       sudo gitlab-ctl reconfigure
       ```

7. **Open Firewall Ports (Optional)**:
   - If you need to open firewall ports for HTTP, HTTPS, and SSH access, run the following commands:

     ```bash
     sudo firewall-cmd --permanent --add-service=http
     sudo firewall-cmd --permanent --add-service=https
     sudo firewall-cmd --permanent --add-service=ssh
     sudo firewall-cmd --reload
     ```

8. **Login & First-Time Setup**:
   - After GitLab is running, visit [http://<your-ec2-public-ip>](http://<your-ec2-public-ip>) to set the root password. You can then log in using:
     - **Username**: root
     - **Password**: the password you just set

9. **Official GitLab References**:
   - For additional details and references, you can refer to the following GitLab documentation:
     - [Install GitLab CE](https://about.gitlab.com/install/)
     - [RHEL / CentOS Install Guide](https://about.gitlab.com/install/#rhelcentos)
     - [System Requirements](https://about.gitlab.com/install/#requirements)
     - [GitLab Config File (`gitlab.rb`)](https://docs.gitlab.com/omnibus/settings/configuration.html)
     - [Backup & Restore GitLab](https://docs.gitlab.com/ee/raketasks/backup_restore.html)
     - [GitLab on AWS](https://docs.gitlab.com/ee/administration/hosts_aws.html)
