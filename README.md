# Web-Application-Firewall-WAF-Home-Lab-Using-SafeLine-DVWA

🛡️ Web Application Firewall (WAF) Home Lab Using SafeLine & DVWA

A Practical Cybersecurity Project for Learning Web App Defense & Attack Simulation

<img width="1857" height="892" alt="Screenshot 2025-11-29 184327" src="https://github.com/user-attachments/assets/5ca80849-7296-4d1d-af51-5f712530c359" />


📌 Overview

This project demonstrates how to build a complete cybersecurity home lab focused on web application security, attack simulation, and defensive architecture using:

-SafeLine WAF

-DVWA (Damn Vulnerable Web App)

-Kali Linux (attack machine)

-Ubuntu Server (web server)

-VirtualBox for virtualization

The lab covers both offensive and defensive security — performing web attacks and observing how the WAF mitigates them.

🎯 Key Objectives

-Deploy DVWA on a LAMP environment

-Simulate real-world attacks (SQL Injection)

-Configure SafeLine WAF for web protection

-Set up VirtualBox networking + DNS

-Apply WAF rules such as IP blocking, rate limiting, and authentication

-Perform log analysis and verify WAF response

🏗️ Lab Architecture
               ┌──────────────────────┐
               │     Legit Users       │
               └──────────┬───────────┘
                          │ Requests
                          ▼
                ┌─────────────────────┐
                │    SafeLine WAF     │
                │  (Reverse Proxy)    │
                └──────────┬──────────┘
                          ▼
                ┌─────────────────────┐
                │   DVWA on Ubuntu    │
                │  Apache + MySQL     │
                └─────────────────────┘

Attack Traffic from Kali Linux  ─────────►  SafeLine WAF  ─────────►  DVWA

🚀 Features Implemented
✔️ Virtualized Security Environment

VirtualBox VMs

Kali Linux (attacker)

Ubuntu Server (victim)

Bridged networking for realistic scenarios

✔️ DVWA Deployment

LAMP stack installation

MySQL database + DVWA configuration

Custom Apache port (8080)

Custom database entries for exploitation

✔️ SafeLine WAF Setup

Automatic installation script

Reverse proxy configuration for DVWA

Domain mapping with /etc/hosts

No SSL involved (HTTP routing only)

✔️ Security Testing

SQL Injection exploitation

WAF event logs & blocked requests

Custom deny rules

Rate-limiting and HTTP flood defense

Authentication gateway (optional)

✔️ DNS Resolution

/etc/hosts domain mapping

Optional BIND DNS if needed

📥 Installation & Setup Guide
1. Install DVWA
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git

2. Install LAMP Stack
sudo apt install apache2 php php-mysql mysql-server -y

3. Configure MySQL
CREATE DATABASE dvwa;
CREATE USER 'dvwa_user'@'localhost' IDENTIFIED BY 'p@ssw0rd';
GRANT ALL ON dvwa.* TO 'dvwa_user'@'localhost';
FLUSH PRIVILEGES;

4. Run DVWA on Port 8080
sudo nano /etc/apache2/ports.conf
# Change: Listen 80 → Listen 8080

5. Connect DVWA to Port 8080, domainname
# Go to /etc/apache2/sites-available
# Create dvwa.conf
# Paste the code below
<VirtualHost *:8080>
    ServerAdmin admin@mylocal.dvwa
    DocumentRoot /var/www/html/DVWA
    ServerName mylocal.dvwa

    <Directory /var/www/html/DVWA>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# Run sudo systemctl a2ensite dvwa.conf

6. Change /etc/hosts file
sudo nano /etc/hosts
# Add : <UbuntuIP> mylocal.dvwa
# Do same to Attack Machine(Kali)

7. Install SafeLine WAF
bash -c "$(curl -fsSLk https://waf.chaitin.com/release/latest/manager.sh)" -- --en

8. Onboard DVWA into SafeLine

Add backend: http://<UbuntuIP>:8080
Add domain: mylocal.dvwa
Add port: 4444


🔥 Attack Demonstration (Kali Linux)

Go to:
http://mylocal.dvwa:4444

Login:
-admin / password

-Set DVWA security to Low

Try SQL Injection payloads:

-admin' OR '1'='1

-View SafeLine WAF logs for blocked events

-Log analysis & threat detection


<img width="1855" height="890" alt="Screenshot 2025-11-29 184240" src="https://github.com/user-attachments/assets/1c6bfaf4-223d-4585-8d1c-802142c8e6f9" />


<img width="1918" height="882" alt="Screenshot 2025-11-29 184434" src="https://github.com/user-attachments/assets/ff24bc1a-16bc-492d-bc90-469e7674fe13" />


🧠 Skills Gained

Cybersecurity fundamentals

Web application security

WAF configuration

Linux administration

MySQL & Apache setup

Attack simulation & defense

DNS & networking
