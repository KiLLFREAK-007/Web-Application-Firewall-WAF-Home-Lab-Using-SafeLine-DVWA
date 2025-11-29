# Web Application Firewall (WAF) Home Lab Using SafeLine & DVWA

🛡️ **Web Application Firewall (WAF) Home Lab Using SafeLine & DVWA**

A practical cybersecurity project for learning web application defense and attack simulation.

<img width="1857" height="892" alt="Screenshot 2025-11-29 184327" src="https://github.com/user-attachments/assets/0e9a6ccc-ad5e-45ca-84d0-7d4668986d14" />


---

## 📌 Overview
This project builds a complete security lab using:

- SafeLine WAF  
- DVWA  
- Kali Linux  
- Ubuntu Server  
- VirtualBox  

The lab demonstrates both attack & defense — performing web attacks and observing WAF protection.

---

## 🏗️ Lab Architecture

```
               ┌──────────────────────┐
               │     Legit Users      │
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
```

---

## 🚀 Features Implemented

### ✔️ Virtualized Security Environment
- VirtualBox VMs  
- Kali Linux (attacker)  
- Ubuntu Server (victim)  
- Bridged networking  

### ✔️ DVWA Deployment
- LAMP installation  
- MySQL database  
- Apache port change to 8080  
- DVWA configuration  

### ✔️ SafeLine WAF Setup
- Automatic installation  
- Reverse proxy setup  
- Domain mapping via `/etc/hosts`  
- No SSL (HTTP only)  

### ✔️ Security Testing
- SQL Injection  
- Event logs  
- Custom deny rules  
- Rate limiting  
- Authentication  

---

## 📥 Installation & Setup Guide

### **1. Install DVWA**
```bash
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
```

### **2. Install LAMP Stack**
```bash
sudo apt install apache2 php php-mysql mysql-server -y
```

### **3. Configure MySQL**
```sql
CREATE DATABASE dvwa;
CREATE USER 'dvwa_user'@'localhost' IDENTIFIED BY 'p@ssw0rd';
GRANT ALL ON dvwa.* TO 'dvwa_user'@'localhost';
FLUSH PRIVILEGES;
```

### **4. Run DVWA on Port 8080**
```bash
sudo nano /etc/apache2/ports.conf
# Change:
# Listen 80 → Listen 8080
```

### **5. Create dvwa.conf - /etc/apache2/sites-available/**
```apache
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
```

Enable site:
```bash
sudo systemctl a2ensite dvwa.conf
```

### **6. Edit /etc/hosts**
```
<UbuntuIP> mylocal.dvwa
```

Do the same on Kali.

### **7. Install SafeLine WAF**
```bash
bash -c "$(curl -fsSLk https://waf.chaitin.com/release/latest/manager.sh)" -- --en
```

### **8. Onboard DVWA in SafeLine**
- Backend: `http://<UbuntuIP>:8080`  
- Domain: `mylocal.dvwa`  
- Port: `4444`  

---

## 🔥 Attack Demonstration (Kali Linux)

Visit:  
```
http://mylocal.dvwa:4444
```

Login:
```
admin / password
```

Set DVWA security to **Low**

Try SQL injection:
```
admin' OR '1'='1
```

Check SafeLine WAF logs.

---

<img width="1855" height="890" alt="Screenshot 2025-11-29 184240" src="https://github.com/user-attachments/assets/f78620e5-5b70-47c7-876c-376f294ae97e" />

<img width="1918" height="882" alt="Screenshot 2025-11-29 184434" src="https://github.com/user-attachments/assets/a955c812-6395-49fb-9717-b6763fa6b163" />


## 🧠 Skills Gained

- Cybersecurity fundamentals  
- Web app security  
- WAF configuration  
- Linux administration  
- MySQL & Apache  
- Attack simulation  
- DNS & networking  

---

