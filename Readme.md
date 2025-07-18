# n8n Docker Setup

This project provides a complete n8n setup using Docker Compose with PostgreSQL database for production-ready workflow automation.

## 🚀 Quick Start

### Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- Git (to clone this repository)

### Setup Instructions

1. **Clone or download this repository**
   ```bash
   git clone <your-repo-url>
   cd n8n
   ```

2. **Create environment file**
   ```bash
   # Create .env file with your configuration
   cp .env.example .env
   # Edit .env with your preferred values
   ```

3. **Start the services**
   ```bash
   docker-compose up -d
   ```

4. **Access n8n**
   - Open your browser and go to: `http://localhost:5678`
   - Login with:
     - Username: `admin`
     - Password: `admin_password_secure`

## 📁 Project Structure

```
n8n/
├── docker-compose.yml    # Main Docker Compose configuration
├── init-data.sh         # PostgreSQL initialization script
├── .env                 # Environment variables (create this)
└── README.md           # This file
```

## 🔧 Configuration

### Environment Variables (.env file)

Create a `.env` file in the project root with the following variables:

```bash
# PostgreSQL Configuration
POSTGRES_USER=n8n_admin
POSTGRES_PASSWORD=n8n_password_secure
POSTGRES_DB=n8n
POSTGRES_NON_ROOT_USER=n8n_user
POSTGRES_NON_ROOT_PASSWORD=n8n_user_password_secure
```

### Default n8n Web Access
- **URL**: http://localhost:5678
- **Username**: admin
- **Password**: admin_password_secure

⚠️ **Security Note**: Change these default credentials in production!

## 🐳 Docker Services

### 1. PostgreSQL Database
- **Image**: `postgres:16`
- **Purpose**: Stores workflows, executions, and user data
- **Volume**: `db_storage` (persistent data)
- **Health Check**: Ensures database is ready before n8n starts

### 2. n8n Application
- **Image**: `docker.n8n.io/n8nio/n8n`
- **Purpose**: Workflow automation platform
- **Volume**: `n8n_storage` (user settings, credentials, encryption keys)
- **Port**: 5678 (accessible at http://localhost:5678)

## 📊 Data Persistence

Your data is safely stored in Docker volumes:

- **`db_storage`**: PostgreSQL database (workflows, executions)
- **`n8n_storage`**: n8n user data (credentials, settings, encryption keys)

⚠️ **Important**: Never delete these volumes unless you want to lose all data!

## 🛠️ Management Commands

### Start services
```bash
docker-compose up -d
```

### Stop services
```bash
docker-compose down
```

### View logs
```bash
# All services
docker-compose logs

# Specific service
docker-compose logs n8n
docker-compose logs postgres
```

### Update n8n version
```bash
# Edit docker-compose.yml to change version tag
# Then run:
docker-compose down
docker-compose pull
docker-compose up -d
```

### Backup data
```bash
# Backup PostgreSQL data
docker-compose exec postgres pg_dump -U n8n_admin n8n > backup.sql

# Backup n8n storage
docker run --rm -v n8n_n8n_storage:/data -v $(pwd):/backup alpine tar czf /backup/n8n-storage-backup.tar.gz -C /data .
```

## 🔒 Security Considerations

1. **Change default passwords** in the `.env` file
2. **Use strong passwords** for production environments
3. **Consider using secrets management** for sensitive data
4. **Restrict network access** in production
5. **Regular backups** of both database and n8n storage

## 🚨 Troubleshooting

### Common Issues

**n8n won't start**
```bash
# Check if PostgreSQL is healthy
docker-compose logs postgres

# Check n8n logs
docker-compose logs n8n
```

**Database connection errors**
- Ensure `.env` file exists and has correct values
- Check if PostgreSQL container is running: `docker-compose ps`

**Permission errors**
- The `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true` should handle this automatically

**Port already in use**
- Change the port in `docker-compose.yml`:
  ```yaml
  ports:
    - 5679:5678  # Use port 5679 instead of 5678
  ```

### Reset Everything
```bash
# Stop and remove everything (⚠️ WARNING: This deletes all data!)
docker-compose down -v
docker-compose up -d
```

## 📚 Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!