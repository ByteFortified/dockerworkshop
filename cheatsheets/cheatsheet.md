# Docker Commands Cheatsheet

A comprehensive Docker commands reference guide for container management, image handling, networking, and more.

## Table of Contents
- [Container Management](#container-management)
- [Image Management](#image-management)
- [Network Management](#network-management)
- [Volume Management](#volume-management)
- [Docker Compose](#docker-compose)
- [System and Maintenance](#system-and-maintenance)
- [Examples and Use Cases](#examples-and-use-cases)
- [Best Practices and Tips](#best-practices-and-tips)

## Container Management

### Run Containers

**Basic container execution**
```bash
docker run nginx
```

**Run in detached mode (background)**
```bash
docker run -d nginx
```

**Run with interactive terminal**
```bash
docker run -it ubuntu bash
```

**Run with port mapping**
```bash
docker run -p 8080:80 nginx
```

**Run with container name**
```bash
docker run --name my-nginx nginx
```

**Run with volume mount**
```bash
docker run -v /host/path:/container/path nginx
```

**Run with environment variables**
```bash
docker run -e ENV_VAR=value nginx
```

**Run and automatically remove when stopped**
```bash
docker run --rm nginx
```

### Manage Container Lifecycle

**List running containers**
```bash
docker ps
```

**List all containers (including stopped)**
```bash
docker ps -a
```

**Stop a container**
```bash
docker stop <container_name_or_id>
```

**Start a stopped container**
```bash
docker start <container_name_or_id>
```

**Restart a container**
```bash
docker restart <container_name_or_id>
```

**Remove a container**
```bash
docker rm <container_name_or_id>
```

**Force remove a running container**
```bash
docker rm -f <container_name_or_id>
```

### Container Inspection

**View container logs**
```bash
docker logs <container_name_or_id>
```

**Follow logs in real-time**
```bash
docker logs -f <container_name_or_id>
```

**Inspect container details**
```bash
docker inspect <container_name_or_id>
```

**View resource usage**
```bash
docker stats <container_name_or_id>
```

**Execute command in running container**
```bash
docker exec -it <container_name_or_id> bash
```

**View running processes in container**
```bash
docker top <container_name_or_id>
```

## Image Management

### Build Images

**Build image from Dockerfile**
```bash
docker build -t myapp:latest .
```

**Build with specific Dockerfile**
```bash
docker build -f Dockerfile.dev -t myapp:dev .
```

**Build with build arguments**
```bash
docker build --build-arg ENV=production -t myapp:prod .
```

### Manage Images

**Pull image from registry**
```bash
docker pull nginx:latest
```

**List local images**
```bash
docker images
```

**Remove an image**
```bash
docker rmi <image_name_or_id>
```

**Inspect image details**
```bash
docker inspect <image_name_or_id>
```

**View image history**
```bash
docker history <image_name_or_id>
```

### Cleanup

**Remove all unused images**
```bash
docker image prune
```

**Remove all stopped containers**
```bash
docker container prune
```

## Network Management

### Network Operations

**List networks**
```bash
docker network ls
```

**Create a network**
```bash
docker network create my-network
```

**Inspect network details**
```bash
docker network inspect my-network
```

**Remove a network**
```bash
docker network rm my-network
```

### Container Networking

**Run container on specific network**
```bash
docker run --network my-network nginx
```

**Connect container to network**
```bash
docker network connect my-network my-container
```

**Disconnect container from network**
```bash
docker network disconnect my-network my-container
```

## Volume Management

### Volume Operations

**List volumes**
```bash
docker volume ls
```

**Create a volume**
```bash
docker volume create my-volume
```

**Inspect volume details**
```bash
docker volume inspect my-volume
```

**Remove a volume**
```bash
docker volume rm my-volume
```

### Using Volumes

**Use named volume**
```bash
docker run -v my-volume:/app/data nginx
```

**Use bind mount**
```bash
docker run -v /host/path:/container/path nginx
```

**Read-only volume**
```bash
docker run -v my-volume:/app/data:ro nginx
```

## Docker Compose

### Basic Operations

**Start services**
```bash
docker-compose up
```

**Start in detached mode**
```bash
docker-compose up -d
```

**Stop services**
```bash
docker-compose down
```

**View service status**
```bash
docker-compose ps
```

**View logs**
```bash
docker-compose logs
```

### Service Management

**Build images**
```bash
docker-compose build
```

**Execute command in service**
```bash
docker-compose exec web bash
```

**Scale services**
```bash
docker-compose up --scale web=3
```

**Follow specific service logs**
```bash
docker-compose logs -f web
```

## System and Maintenance

### System Information

**Docker version**
```bash
docker version
```

**System information**
```bash
docker info
```

**Disk usage**
```bash
docker system df
```

### Cleanup Commands

**Remove all unused data**
```bash
docker system prune
```

**Remove all unused resources including volumes**
```bash
docker system prune -a --volumes
```

## Examples and Use Cases

### Development Environment

**Node.js development with hot reload**
```bash
docker run -v $(pwd):/app -p 3000:3000 node:14
```

**Python development**
```bash
docker run -v $(pwd):/code -p 8000:8000 python:3.9
```

**Database with persistent storage**
```bash
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=secret -v mysql_data:/var/lib/mysql mysql:8.0
```

### Multi-Container Setup

**Web application stack**
```bash
docker run -d --name webapp -p 80:80 nginx
docker run -d --name database -e POSTGRES_PASSWORD=secret postgres:13
docker run -d --name cache redis:6.0
```

### Debugging and Troubleshooting

**Interactive debugging session**
```bash
docker run -it --rm node:14 bash
```

**Copy files to container**
```bash
docker cp file.txt container:/path/
```

**Copy files from container**
```bash
docker cp container:/path/file.txt ./
```

**Check container IP address**
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name
```

## Best Practices and Tips

### Container Management Tips

1. **Always use `--rm` for temporary containers** to avoid container clutter
   ```bash
   docker run --rm -it ubuntu bash
   ```

2. **Use meaningful container names** for easier management
   ```bash
   docker run --name my-web-server nginx
   ```

3. **Run in detached mode for services** that need to run in background
   ```bash
   docker run -d --name database postgres
   ```

### Image Management Tips

4. **Use specific version tags** instead of `latest` for production
   ```bash
   docker run nginx:1.21
   ```

5. **Leverage multi-stage builds** for smaller final images
   ```dockerfile
   # In Dockerfile
   FROM node:14 as builder
   # Build steps...
   FROM nginx:alpine
   COPY --from=builder /app/dist /usr/share/nginx/html
   ```

### Security Best Practices

6. **Don't run as root** when possible
   ```dockerfile
   # In Dockerfile
   USER node
   ```

7. **Use read-only volumes** for data that shouldn't change
   ```bash
   docker run -v config:/app/config:ro myapp
   ```

8. **Avoid using `--privileged`** unless absolutely necessary

### Performance Tips

9. **Use `.dockerignore`** to exclude unnecessary files from build context
   ```dockerignore
   node_modules
   .git
   *.log
   .env
   ```

10. **Leverage layer caching** by ordering Dockerfile commands properly
    ```dockerfile
    # Install dependencies first (cached layer)
    COPY package.json .
    RUN npm install
    
    # Then copy source code (changes more frequently)
    COPY . .
    ```

### Development Tips

11. **Use bind mounts for development** to enable live reload
    ```bash
    docker run -v $(pwd):/app -p 3000:3000 node:14
    ```

12. **Use named volumes for production data** persistence
    ```bash
    docker run -v db_data:/var/lib/postgresql postgres
    ```

13. **Clean up regularly** to free up disk space
    ```bash
    docker system prune -f
    ```

### Networking Tips

14. **Use custom networks** for multi-container applications
    ```bash
    docker network create app-network
    docker run --network app-network --name web nginx
    docker run --network app-network --name db postgres
    ```

15. **Use Docker Compose** for complex multi-service applications
    ```yaml
    # docker-compose.yml
    version: '3.8'
    services:
      web:
        image: nginx
        ports:
          - "80:80"
      db:
        image: postgres
        environment:
          POSTGRES_PASSWORD: secret
    ```

### Monitoring and Debugging Tips

16. **Use `docker stats`** to monitor resource usage
    ```bash
    docker stats
    ```

17. **Follow logs during development** for real-time debugging
    ```bash
    docker logs -f container_name
    ```

18. **Use `docker exec`** for troubleshooting inside containers
    ```bash
    docker exec -it container_name bash
    ```

### General Tips

19. **Use Docker Desktop** for easy GUI management on Windows/Mac
20. **Learn Dockerfile best practices** for efficient images
21. **Use health checks** in production containers
22. **Set resource limits** for production deployments
23. **Use Docker Scout** for image vulnerability scanning
24. **Regularly update base images** for security patches
25. **Use docker scan** to check images for vulnerabilities

Remember: Practice these commands regularly to build muscle memory and always test in development environments before running in production!

---

**Happy Dockering! 🐳**