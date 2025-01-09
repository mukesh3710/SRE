# Docker 

## Objectives

### 1. What are Containers?
- Containers are lightweight, portable units of software that package code, dependencies, and runtime environment.
- They isolate applications, ensuring consistent behavior across different systems.
- Built on OS-level virtualization, sharing the host kernel.

### 2. What is Docker?
- Docker is a platform to build, ship, and run containers.
- It simplifies the process of creating and managing containers.
- Provides tools for container management, including the Docker Engine and Docker CLI.

### 3. Why Do You Need Docker?
- Simplifies application deployment and ensures consistency across development, testing, and production.
- Efficient resource usage compared to virtual machines.
- Facilitates microservices architecture by isolating individual services.

### 4. What Can Docker Do?
- Create reproducible development environments.
- Enable CI/CD pipelines.
- Simplify scaling and load balancing.
- Package applications with all dependencies for portability.

---

## Running Docker Containers

### Steps to Run Containers:
1. Pull an image: `docker pull <image>`
2. Start a container: `docker run <options> <image>`
3. Manage containers:
   - List running containers: `docker ps`
   - Stop a container: `docker stop <container_id>`
   - Remove a container: `docker rm <container_id>`

---

## Creating a Docker Image

### Steps to Create a Docker Image:
1. Create a `Dockerfile`:
   ```dockerfile
   FROM <base_image>
   COPY . /app
   WORKDIR /app
   RUN <build_commands>
   CMD ["<entry_point>"]
   ```
2. Build the image: `docker build -t <image_name> .`
3. Verify the image: `docker images`

---

## Networks in Docker
- **Bridge Network**: Default for standalone containers, provides NAT connectivity.
- **Host Network**: Shares the host network stack.
- **Overlay Network**: For multi-host container communication in Swarm mode.
- **Custom Network**: User-defined, allowing finer control.

Commands:
- List networks: `docker network ls`
- Create a network: `docker network create <network_name>`

---

## Docker Compose
- Simplifies multi-container application management.
- Use `docker-compose.yml` to define services, networks, and volumes.

Example:
```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:80"
    networks:
      - app_network
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
    networks:
      - app_network
networks:
  app_network:
```

---

## Docker Concepts in Depth
- **Volumes**: Persistent data storage outside the container.
  - Create: `docker volume create <name>`
  - Inspect: `docker volume inspect <name>`
- **Images and Layers**: Docker images are built using layers, optimizing storage and builds.
- **Registry**: Docker Hub or private registries store and share images.

---

## Docker for Windows/Mac
- Use Docker Desktop for an easy GUI and CLI integration.
- Ensure virtualization is enabled on your machine.
- Provides Kubernetes support for local testing.

---

## Docker Swarm
- Native clustering and orchestration tool for Docker.
- Deploy and manage multi-container applications.

Commands:
- Initialize Swarm: `docker swarm init`
- Add nodes: `docker swarm join <token>`
- Deploy services: `docker service create <options>`

---

## Docker vs Kubernetes
- **Docker**: Focuses on containerization and single-node management.
- **Kubernetes**: Advanced orchestration tool for large-scale, multi-node deployments.
- Use Docker with Kubernetes as the container runtime.

---

## Production-Ready Example: Docker + Kubernetes

### Application Overview
Deploy a Node.js application in a Kubernetes cluster with multiple replicas.

### Dockerfile for Node.js App
```dockerfile
FROM node:14
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

### Kubernetes Manifest Files

#### Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: <dockerhub_username>/node-app:latest
        ports:
        - containerPort: 3000
```

#### Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

### Steps to Deploy
1. Build and push the Docker image:
   ```bash
   docker build -t <dockerhub_username>/node-app:latest .
   docker push <dockerhub_username>/node-app:latest
   ```
2. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```
3. Verify Deployment:
   ```bash
   kubectl get pods
   kubectl get svc
   ```

The application will be available via the external IP of the LoadBalancer.
