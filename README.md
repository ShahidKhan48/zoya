# Docker Projects Collection

This repository contains various Docker applications and configurations for learning and development purposes.

## Project Structure
hi
```
docker-projects/
├── python-app/          # Flask Python application
├── react-app/           # React/Node.js application  
├── java-app/            # Java HTTP server application
├── static-app/          # Static HTML/CSS/JS application
├── docker-compose/      # Multi-tier application examples
└── README.md           # This file
```

## Applications Overview

### 1. Python Flask App (Port 5000)
- **Location**: `python-app/`
- **Technology**: Python Flask
- **Port**: 5000
- **Endpoints**: `/` (home), `/health` (health check)

### 2. React/Node.js App (Port 3000)
- **Location**: `react-app/`
- **Technology**: Node.js + Express + React
- **Port**: 3000
- **Endpoints**: `/` (home), `/api/health` (API health check)

### 3. Java App (Port 8080)
- **Location**: `java-app/`
- **Technology**: Java HTTP Server
- **Port**: 8080
- **Endpoints**: `/` (home), `/health` (health check)

### 4. Static App (Port 80)
- **Location**: `static-app/`
- **Technology**: HTML/CSS/JavaScript + Nginx
- **Port**: 80
- **Features**: Interactive frontend with styling

## Docker Images

### Pre-built Images Used
- `python:3.9-slim` - Lightweight Python runtime
- `node:18-alpine` - Node.js Alpine Linux
- `openjdk:17-jdk-slim` - Java Development Kit
- `nginx:alpine` - Nginx web server
- `mysql:8.0` - MySQL database server

### Building Custom Images

```bash
# Build Python app
cd python-app
docker build -t my-python-app .

# Build React app
cd react-app
docker build -t my-react-app .

# Build Java app
cd java-app
docker build -t my-java-app .

# Build Static app
cd static-app
docker build -t my-static-app .
```

## Container Commands

### Basic Container Operations

```bash
# Run containers individually
docker run -d -p 5000:5000 --name python-container my-python-app
docker run -d -p 3000:3000 --name react-container my-react-app
docker run -d -p 8080:8080 --name java-container my-java-app
docker run -d -p 80:80 --name static-container my-static-app

# Stop containers
docker stop python-container react-container java-container static-container

# Remove containers
docker rm python-container react-container java-container static-container

# View running containers
docker ps

# View all containers
docker ps -a

# View container logs
docker logs <container-name>

# Execute commands in running container
docker exec -it <container-name> /bin/bash

# Inspect container details
docker inspect <container-name>
```

### Image Management

```bash
# List images
docker images

# Remove images
docker rmi <image-name>

# Pull images from registry
docker pull <image-name>

# Push images to registry
docker push <image-name>

# Tag images
docker tag <source-image> <target-image>

# Build with custom tag
docker build -t <tag-name> .

# Remove unused images
docker image prune
```

### Container Resource Management

```bash
# Run with resource limits
docker run -d --memory="512m" --cpus="1.0" -p 5000:5000 my-python-app

# Monitor resource usage
docker stats

# View container processes
docker top <container-name>
```

## Docker Networking

### Network Types
- **bridge** (default): Isolated network for containers
- **host**: Uses host machine's network
- **none**: No networking
- **overlay**: Multi-host networking for swarm

### Network Commands

```bash
# List networks
docker network ls

# Create custom network
docker network create --driver bridge my-network

# Run container with custom network
docker run -d --network my-network --name app1 my-python-app

# Connect container to network
docker network connect my-network <container-name>

# Disconnect container from network
docker network disconnect my-network <container-name>

# Inspect network
docker network inspect my-network

# Remove network
docker network rm my-network

# Create network with custom subnet
docker network create --driver bridge --subnet=172.20.0.0/16 custom-net
```

### Container Communication

```bash
# Containers on same network can communicate by name
docker run -d --network my-network --name db mysql:8.0
docker run -d --network my-network --name app my-python-app
# App can connect to database using hostname 'db'
```

## Docker Volumes

### Volume Types
- **Named volumes**: Managed by Docker
- **Bind mounts**: Direct host path mapping
- **tmpfs mounts**: Temporary filesystem in memory

### Volume Commands

```bash
# List volumes
docker volume ls

# Create named volume
docker volume create my-data

# Run container with named volume
docker run -d -v my-data:/app/data my-python-app

# Run container with bind mount
docker run -d -v /host/path:/container/path my-python-app

# Run container with tmpfs mount
docker run -d --tmpfs /tmp my-python-app

# Inspect volume
docker volume inspect my-data

# Remove volume
docker volume rm my-data

# Remove unused volumes
docker volume prune

# Backup volume data
docker run --rm -v my-data:/data -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz -C /data .

# Restore volume data
docker run --rm -v my-data:/data -v $(pwd):/backup alpine tar xzf /backup/backup.tar.gz -C /data
```

### Volume Examples

```bash
# Database with persistent storage
docker run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=password \
  -v mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  mysql:8.0

# Development with code sync
docker run -d \
  --name dev-app \
  -v $(pwd):/app \
  -p 5000:5000 \
  my-python-app

# Shared volume between containers
docker volume create shared-data
docker run -d --name writer -v shared-data:/data alpine
docker run -d --name reader -v shared-data:/data alpine
```

## Docker Compose Multi-Tier Applications

### 2-Tier Application (Web + Database)
```bash
cd docker-compose
docker-compose -f 2-tier-app.yml up -d
```
- **Components**: Python Flask app + MySQL database
- **Access**: http://localhost:5000

### 3-Tier Application (Frontend + Backend + Database)
```bash
cd docker-compose
docker-compose -f 3-tier-app.yml up -d
```
- **Components**: React frontend + Python backend + MySQL database
- **Access**: http://localhost:3000

### 4-Tier Application (Load Balancer + Frontend + Backend + Database)
```bash
cd docker-compose
docker-compose -f 4-tier-app.yml up -d
```
- **Components**: Nginx proxy + React frontend + Python backend + MySQL database
- **Access**: http://localhost:80

### Docker Compose Commands

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs

# Scale services
docker-compose up -d --scale backend=3

# Rebuild services
docker-compose up -d --build

# View service status
docker-compose ps

# Execute command in service
docker-compose exec <service-name> /bin/bash

# View service logs
docker-compose logs <service-name>

# Remove volumes with services
docker-compose down -v
```

## Quick Start Guide

1. **Clone and navigate to project**:
   ```bash
   cd docker-projects
   ```

2. **Build all applications**:
   ```bash
   # Build each app
   docker build -t my-python-app python-app/
   docker build -t my-react-app react-app/
   docker build -t my-java-app java-app/
   docker build -t my-static-app static-app/
   ```

3. **Run individual applications**:
   ```bash
   docker run -d -p 5000:5000 my-python-app    # Python app
   docker run -d -p 3000:3000 my-react-app     # React app
   docker run -d -p 8080:8080 my-java-app      # Java app
   docker run -d -p 80:80 my-static-app        # Static app
   ```

4. **Or run multi-tier application**:
   ```bash
   cd docker-compose
   docker-compose -f 3-tier-app.yml up -d
   ```

5. **Access applications**:
   - Python Flask: http://localhost:5000
   - React/Node.js: http://localhost:3000
   - Java: http://localhost:8080
   - Static: http://localhost:80

## Troubleshooting

### Common Issues

1. **Port already in use**:
   ```bash
   # Find process using port
   lsof -i :5000
   # Kill process or use different port
   docker run -d -p 5001:5000 my-python-app
   ```

2. **Container won't start**:
   ```bash
   # Check logs
   docker logs <container-name>
   # Check if image exists
   docker images
   ```

3. **Network connectivity issues**:
   ```bash
   # Check networks
   docker network ls
   # Inspect network
   docker network inspect bridge
   ```

4. **Volume mount issues**:
   ```bash
   # Check volume exists
   docker volume ls
   # Check permissions
   ls -la /host/path
   ```

### Cleanup Commands

```bash
# Remove all stopped containers
docker container prune

# Remove all unused images
docker image prune -a

# Remove all unused volumes
docker volume prune

# Remove all unused networks
docker network prune

# Complete cleanup
docker system prune -a --volumes
```

## Best Practices

1. **Use multi-stage builds** for smaller images
2. **Don't run as root** in containers
3. **Use .dockerignore** to exclude unnecessary files
4. **Pin image versions** instead of using 'latest'
5. **Use health checks** for better monitoring
6. **Limit container resources** to prevent resource exhaustion
7. **Use secrets management** for sensitive data
8. **Regular security updates** for base images

## Security Considerations

- Never include secrets in Dockerfiles
- Use official base images when possible
- Regularly update base images
- Scan images for vulnerabilities
- Use non-root users in containers
- Limit container capabilities
- Use read-only filesystems when possible
