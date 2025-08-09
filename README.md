# CopyParty Personal Server

Personal file server running on Proxmox container as Nextcloud replacement.

## Features

- Personal file management with web interface
- Resumable uploads/downloads
- Thumbnails for images/videos
- Audio/video streaming
- Full-text search
- User accounts and permissions
- Mobile-friendly interface

## Quick Setup

### 1. Clone Repository
```bash
git clone <your-repo-url> copyparty-personal
cd copyparty-personal
```

### 2. Create Directories
```bash
mkdir -p data/{personal,shared,admin} config
```

### 3. Configure Users
Edit `copyparty.conf` and update passwords:
```bash
# Generate password hash
python3 -c "from copyparty.pwhash import hash_pwd; print(hash_pwd('your-password'))"
```

### 4. Start Service
```bash
docker-compose up -d
```

### 5. Nginx Proxy Manager Setup
- Add new proxy host
- Domain: your-domain.com
- Forward to: container-ip:3923
- Enable websockets
- SSL certificate setup

## Default Access

- **URL**: http://container-ip:3923
- **Admin User**: admin / changeme123!
- **Regular User**: user / userpass456!

⚠️ **CHANGE DEFAULT PASSWORDS IMMEDIATELY**

## Volumes

- `/personal` - Private user files (admin, user access)
- `/shared` - Shared files (public read, user write)
- `/admin` - Admin-only area

## Reverse Proxy Headers

For proper IP logging with reverse proxy, ensure these headers:
```
X-Forwarded-For
X-Real-IP
X-Forwarded-Proto
```

## Backup

Important directories to backup:
- `./data/` - All user files
- `./config/` - Database and logs
- `./copyparty.conf` - Configuration

## Logs

- Application: `./config/copyparty.log`
- Access: `./config/access.log`

## Troubleshooting

### Container won't start
```bash
docker-compose logs copyparty-personal
```

### Permission issues
```bash
sudo chown -R 1000:1000 data/ config/
```

### Reset admin password
1. Stop container
2. Edit `copyparty.conf`
3. Generate new hash: `python3 -c "from copyparty.pwhash import hash_pwd; print(hash_pwd('newpass'))"`
4. Start container