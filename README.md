# GitLab Docker Compose Setup

This project contains a Docker Compose configuration for deploying GitLab Community Edition with GitLab Runner.

## 🚀 Components

- **GitLab CE** (Community Edition) - version 18.2.0
- **GitLab Runner** - version 18.2.0
- **Let's Encrypt** - automatic SSL certificate provisioning

## 📋 Requirements

- Docker
- Docker Compose
- Domain name pointing to your server
- Open ports: 80, 443, 22 (or configured SSH port)

## ⚙️ Setup

### 1. Create Environment Variables File

Create a `.env` file in the project root directory:

```bash
# Main settings
GITLAB_DOMAIN=gitlab.yourdomain.com
GITLAB_SSH_PORT=2222

# Let's Encrypt
LETSENCRYPT_EMAIL=admin@yourdomain.com
```

## 🚀 Running

### Start All Services

```bash
docker compose up -d
```

### View Logs

```bash
# All services
docker compose logs -f

# GitLab only
docker compose logs -f gitlab

# GitLab Runner only
docker compose logs -f gitlab-runner
```

### Stop Services

```bash
docker compose down
```

## 🔧 GitLab Runner Configuration

After starting GitLab, you need to register the GitLab Runner:

```bash
# Enter GitLab Runner container
docker compose exec gitlab-runner bash

# Register runner
gitlab-runner register
```

During registration you will need:
- GitLab URL: `https://gitlab.yourdomain.com`
- Registration token (find in GitLab: Admin Area → Runners)
- Runner description
- Tags (e.g.: `docker,linux`)
- Executor: `docker`
- Default Docker image: `alpine:latest`

## 📁 Data Structure

The project uses named Docker volumes for data storage:

- `gitlab-config` - GitLab configuration
- `gitlab-logs` - GitLab logs
- `gitlab-data` - GitLab data (repositories, database)
- `gitlab-runner-config` - GitLab Runner configuration

## 🔐 Security

### Initial Setup

1. After first startup, wait for GitLab initialization to complete (may take 5-10 minutes)
2. Open `https://gitlab.yourdomain.com` in your browser
3. Set password for `root` user
4. Log in and configure additional security settings

### Security Recommendations

- Regularly update GitLab and GitLab Runner
- Set up data backup
- Use strong passwords
- Enable two-factor authentication
- Restrict access to administrative functions

## 🔄 Updates

### Updating GitLab

1. Stop services:
   ```bash
   docker compose down
   ```

2. Update version in `docker compose.yml`

3. Start services:
   ```bash
   docker compose up -d
   ```

### Backup

```bash
# Create backup
docker compose exec gitlab gitlab-backup create

# Backup will be saved in gitlab-data volume
```

## 🐛 Troubleshooting

### Check Service Status

```bash
docker compose ps
```

### View Error Logs

```bash
docker compose logs gitlab | grep -i error
```

### Check Availability

```bash
# Check HTTP
curl -I http://gitlab.yourdomain.com

# Check HTTPS
curl -I https://gitlab.yourdomain.com
```

### Reset Root Password

```bash
docker compose exec gitlab gitlab-rake "gitlab:password:reset[root]"
```

## 📊 Monitoring

### Check Resource Usage

```bash
docker stats
```

### Check Volume Sizes

```bash
docker system df -v
```

## 🔗 Useful Links

- [Official GitLab Documentation](https://docs.gitlab.com/)
- [GitLab Docker Images](https://hub.docker.com/r/gitlab/gitlab-ce)
- [GitLab Runner Documentation](https://docs.gitlab.com/runner/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)

## 📝 License

This project uses GitLab Community Edition, which is distributed under the MIT license.

## 🤝 Support

If you encounter issues:

1. Check service logs
2. Verify DNS configuration
3. Check port availability
4. Refer to official GitLab documentation
