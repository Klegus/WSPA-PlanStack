# XLSX-Refresher-Stack

## 📋 Requirements
- Docker and Docker Compose
- Windows (version with WSL2) / Any Linux Distro
- Administrator privileges
- Git

## 🚀 Quick Installation

1. Clone the repository and navigate to the project directory:
```bash
git clone <repository-url>
cd <project-directory>
```

2. Create the directory structure:
```powershell
mkdir nginx\conf nginx\auth nginx\logs fail2ban\data fail2ban\log wg-easy-data
```

3. Create a `.env` file with the appropriate variables:
```env
MONGO_ROOT_USERNAME=admin
MONGO_ROOT_PASSWORD=your_password
EMAIL=your_email
PASSWORD=your_password
OPENROUTER_API_KEY=your_key
SELECTED_MODEL=your_model
DISCORD_WEBHOOK_URL=your_webhook
MOODLE_URL=your_moodle_url
DEV=false
MONGO_DB=your_db_name
WERKZEUG_RUN_MAIN=true
ENABLE_COMPARER=true
COMPARER=true
```

## ⚙️ Configuration

### 1. DNS and Hosts

Add entries to the hosts file (as administrator):
```powershell
If using windows add to 'hosts' file
127.0.0.1 wg-admin.local
127.0.0.1 panel.backend.local
```

### 2. Basic Auth

Generate a password for basic auth (LOGIN, PASSWORD):
```powershell
docker run --rm httpd:2.4-alpine htpasswd -Bbn LOGIN PASSWORD > .\nginx\auth\.htpasswd
```

### 3. Firewall

Open the required ports in Windows Firewall:
```powershell
WireGuard VPN Inbound TCP  51820
Nginx Proxy Inbound TCP  51821 
```

## 🔒 Launch

1. Launch the stack:
```powershell
docker-compose up -d
```

2. Check the status:
```powershell
docker-compose ps
docker logs nginx-proxy
docker logs wg-easy
```

## 🌐 Access

### WireGuard Panel
- URL: http://wg-admin.local:51821
- Username: LOGIN
- Password: PASSWORD

### Backend Admin Panel
- URL: http://panel.backend.local:51821
- Username: LOGIN
- Password: PASSWORD

### Frontend
- URL: http://localhost:88

## 📱 WireGuard Client Configuration

1. Download WireGuard for your system:
    - Windows: [WireGuard](https://download.wireguard.com/windows-client/wireguard-installer.exe)
    - macOS: `brew install wireguard-tools`
    - Linux: `sudo apt install wireguard`

2. Add a client through the WireGuard panel or use the command:
```powershell
docker exec -it wg-easy wg-easy client add my-device
```

3. Import the generated configuration into the WireGuard client.

## 🔐 Security

The stack includes the following security measures:
- Fail2ban for brute force attack protection
- Basic Auth for the admin panel
- Access to the panel only through VPN
- Docker network isolation
- Rate limiting in Nginx

## 📝 Logs

Checking logs:
```powershell
# Nginx
docker logs nginx-proxy

# WireGuard
docker logs wg-easy

# Fail2ban
docker logs fail2ban
```

## 🛠 Troubleshooting

### Cannot connect to VPN
1. Check if UDP port 51820 is open:
```powershell
Test-NetConnection -ComputerName localhost -Port 51820
```

2. Check WireGuard logs:
```powershell
docker logs wg-easy
```

### Problem accessing the panel
1. Check service availability:
```powershell
curl -v -u admin:adam http://panel.backend.local:51821
```

2. Check Nginx logs:
```powershell
docker logs nginx-proxy
```

### Unblock banned IP
```powershell
docker exec fail2ban fail2ban-client unban <ip>
```

## ⚠️ Notes
- Ensure that ports 51820 (UDP) and 51821 (TCP) are not used by other applications
- Keep all passwords and keys in a secure place
- Regularly check logs for unauthorized access attempts
- Remember to backup the WireGuard configuration

## 📞 Support
In case of problems:
1. Check the logs of the appropriate container
2. Ensure all ports are available
3. Check the permissions of configuration files
4. Contact the system administrator