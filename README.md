# 🔒 Debian Server Setup and Hardening with Nagios Monitoring and Penetration Testing

This project focuses on setting up and hardening a Debian server, implementing security measures, monitoring server performance in real-time using Nagios, and conducting penetration tests to assess and enhance security.

---

## 📋 Project Objectives

- **Install and Configure Debian Server:** Set up and configure a Debian server following best practices.
- **Implement Security Measures:** Apply security measures, including robust firewall configuration and SSH key authentication, to harden the server.
- **Monitor Server Performance:** Use Nagios to monitor server performance and security in real-time, enhancing alerting and detection capabilities.
- **Conduct Penetration Testing:** Perform penetration testing to evaluate the server’s security by simulating various attack scenarios.

---

## 🛠 Prerequisites

- Operating System: Debian
- Basic Knowledge of: Linux administration, network security, and system monitoring.

---

## 🔑 Key Elements

1. **Installation and Configuration:**
   - Setting up the Debian server and performing basic configuration.
   - Installing necessary services like Apache.
2. **Server Hardening:**
   - Configuring firewall rules using UFW (Uncomplicated Firewall).
   - Implementing SSH key authentication and disabling password authentication and root login.
3. **Monitoring:**
   - Installing and configuring Nagios for comprehensive, real-time performance and security monitoring.
4. **Penetration Testing:**
   - Brute-Force Attack: Assess the effectiveness of SSH key authentication.
   - Firewall Bypass: Evaluate firewall’s ability to prevent unauthorized access.
   - Denial of Service (DoS): Test server stability under extreme traffic conditions.

---

## 🚀 Steps to Implement

### 1. Set Up the Debian Server

1.1. Install Debian on a virtual or physical machine using the official ISO.

1.2. Configure basic settings such as partitioning and network setup.

1.3. Select software packages:
   - Ensure to select "SSH server" for remote access during installation.
   - Alternatively, install the SSH server manually after installation:
     ```bash
     sudo apt update
     sudo apt install openssh-server
     sudo systemctl start ssh
     sudo systemctl enable ssh
     sudo systemctl status ssh
     ```

1.4. Configure the network, install necessary packages, and secure the services:
   - Update the system:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```
   - Install Apache:
     ```bash
     sudo apt install apache2 -y
     ```
   - Ensure Apache is running:
     ```bash
     sudo systemctl start apache2
     sudo systemctl enable apache2
     ```

### 2. Apply Server Hardening Techniques

2.1. Set up UFW for firewall management:
     ```bash
     sudo apt install ufw -y
     sudo ufw allow OpenSSH
     sudo ufw allow 80/tcp  # For HTTP
     sudo ufw allow 443/tcp  # For HTTPS
     sudo ufw enable
     ```

2.2. Configure SSH key-based authentication, and disable password-based login and root access:
   - Generate SSH keys on your local machine:
     ```bash
     ssh-keygen -t rsa -b 4096
     ```
   - Copy the public key to the server:
     ```bash
     ssh-copy-id username@your_server_ip
     ```
   - Verify key authentication:
     ```bash
     ssh username@ip_address
     ```
   - Disable password authentication and root login in /etc/ssh/sshd_config:
     ```bash
     sudo nano /etc/ssh/sshd_config
     # Set PasswordAuthentication no
     # Set PermitRootLogin no
     sudo systemctl restart ssh
     ```

### 3. Install and Configure Nagios

3.1. Install Nagios and configure it to monitor server performance and security:
   - Install prerequisites:
     ```bash
     sudo apt install apache2 libapache2-mod-php php
     sudo apt install wget unzip zip autoconf gcc libc6 make apache2-utils libgd-dev
     ```
   - Download and install Nagios:
     ```bash
     sudo useradd nagios
     sudo usermod -aG nagios www-data
     wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.6.tar.gz
     tar xzf nagios-4.4.6.tar.gz
     cd nagios-4.4.6
     sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
     sudo make all
     sudo make install
     sudo make install-init
     sudo make install-commandmode
     sudo make install-config
     sudo make install-webconf
     ```
   - Create a Nagios user:
     ```bash
     sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
     sudo a2enmod cgi
     sudo systemctl restart apache2
     ```
   - Start Nagios and enable it to start at boot:
     ```bash
     sudo systemctl start nagios
     sudo systemctl enable nagios
     ```

3.2. Set up alerting and monitoring for key services and security events:



### 4. Conduct Penetration Testing

4.1. Simulate brute-force attacks using Hydra to test SSH key authentication:
```bash

```

5.2. Perform firewall bypass tests with Nmap to assess the firewall's effectiveness:
```bash

```

5.3. Execute DoS attacks using hping3 and analyze the server's response to extreme traffic conditions:
```bash

```

---

## Documentation and Mitigation Actions

### Documentation

Throughout the project, all key activities were meticulously documented, including:

1. Server Hardening:
   - SSH authentication logs before and after implementing key-based authentication (`sshd_log`).
   - Firewall configuration changes before and after hardening (`ufw_before.txt` and `ufw_after.txt`).
   - Nmap scans showing firewall effectiveness (`nmap_before.txt` and `nmap_after.txt`).

2. Penetration Testing:
   - Hydra output demonstrating the effectiveness of SSH key authentication in preventing unauthorized access (`hydra_output_before-txt.png` and `hydra_output_after-txt.png`).
   - Logs and scan results confirming the reduction in unauthorized access attempts due to robust firewall configuration ([`unauthorized_access.txt`](https://drive.google.com/file/d/1qa8Ozu2SyQxFSVn-nVVYB-pBdon_MVBN/view?usp=sharing)).
   - Screenshots from the DoS attack simulation, showing the server's resilience (DoS_attack.png).

3. Monitoring:
   - Nagios configuration files (`nagios.cfg`, `localhost.cfg`, `contacts.cfg`, `commands.cfg`) and logs (`nagios_warnings_before.txt` and `nagios_warnings_after.txt`) displaying improved alert detection and monitoring capabilities.

### Mitigation Actions

Based on the penetration testing results, the following mitigation actions were taken:

1. Brute-Force Attack Mitigation: SSH key-based authentication was fully implemented, resulting in a 100% reduction in successful brute-force attempts. The server’s `sshd_config` was updated to disable password-based authentication entirely.

2. Firewall Hardening: UFW was configured to block unauthorized access attempts. Key settings include active status with logging enabled, and rules to limit access to essential services only, achieving an 85% reduction.

3. DoS Attack Mitigation: Although the server demonstrated good resilience during DoS attack simulations, further mitigation steps are required to enhance its stability under extreme conditions:

   - Rate Limiting: Implemented rate limiting on the web server to mitigate DoS attacks more effectively.
   - Resource Monitoring: Set up additional monitoring using Nagios to detect unusual spikes in traffic and resource usage, allowing for quicker response to potential DoS attempts.
   - Manual Mitigation: Regular review and manual tuning of the firewall settings and server resources are recommended to adapt to evolving threats.

4. Ongoing Monitoring: Nagios monitoring configuration was enhanced, resulting in a 50% increase in detected alerts. Continuous monitoring is in place to ensure prompt detection and response to any new threats.

---

## 📊 Results and Documentation

- **Analysis Results**: The project demonstrated a significant enhancement in server security and performance. The improvements in monitoring and penetration testing contributed to a more resilient and secure server environment.

- **Qualitative Results**:

  1. The Debian server was effectively hardened and secured against common attack vectors.
  2. Enhanced monitoring capabilities were achieved through Nagios, providing real-time alerts and detailed insights.
  3. Penetration testing validated the effectiveness of security measures, with successful mitigation of identified vulnerabilities.

- **Quantitative Results**:

  1. Achieved a 100% reduction in successful brute-force login attempts by implementing SSH key authentication.
  2. Reduced unauthorized access attempts by 85% through robust firewall configuration.
  3. Improved alerting and detection capabilities by enhancing Nagios configuration, resulting in a 50% increase in detected alerts.

---

## 📝 Conclusion

The project successfully achieved the goal of configuring a robust, secure, and monitored server environment. The enhancements in security posture and performance monitoring significantly improved the resilience and reliability of the server, making it well-prepared to handle potential security threats.

## ⚠️ Additional Notes

- Regular Assessments: Conduct regular vulnerability assessments and update the system and security tools to maintain a high level of security.
- Continuous Monitoring: Implement continuous monitoring to promptly detect and respond to new threats.
