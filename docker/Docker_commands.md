````markdown
# 🚀 Docker Hands-On Lab (Docker Playground Edition)

This hands-on guide is designed for use in [Docker Playground](https://labs.play-with-docker.com/) so you can run everything without local setup.  

---

## 1️⃣ What and Why of Containers
Containers are lightweight, portable, and efficient compared to traditional deployments.  

### 🛠 Hands-On
```bash
# Check Docker version
docker version

# Show system-wide information
docker info

# Run your first container
docker run hello-world
````

👉 **Observation**: The container runs, executes, and exits – showing how lightweight containers are.

---

## 2️⃣ Difference between VMs and Containers

* **VMs**: Full OS virtualization (heavyweight)
* **Containers**: Share host OS kernel (lightweight)

### 🛠 Hands-On

```bash
# Run Ubuntu container interactively
docker run -it ubuntu bash

# Inside container, check OS details
cat /etc/os-release

# List processes inside container
ps aux

# Exit container
exit

# Check running containers
docker ps

# Check all containers (including stopped ones)
docker ps -a

# Show live container resource usage
docker stats
```

👉 **Observation**: Containers start in seconds and consume fewer resources compared to VMs.

---

## 3️⃣ Docker Architecture and Components

* **Docker Engine (Daemon)**: Runs containers
* **Docker CLI**: Command-line client
* **Docker Hub / Registry**: Stores and shares images

### 🛠 Hands-On

```bash
# Check Docker background process
ps -ef | grep docker

# List Docker networks
docker network ls

# List Docker volumes
docker volume ls

# Inspect details of Docker Engine
docker info
```

---

## 4️⃣ Image Distribution using Docker Hub

Docker Hub is like GitHub, but for container images.

### 🛠 Hands-On

```bash
# Pull official Nginx image
docker pull nginx

# List local images
docker images

# Run Nginx container (detached, map port 8080 → 80)
docker run -d --name mynginx -p 8080:80 nginx

# List running containers
docker ps

# Test in Playground → Open Port 8080
curl http://localhost:8080

# Stop container
docker stop mynginx

# Restart container
docker start mynginx

# Remove container
docker rm -f mynginx

# Remove image
docker rmi nginx
```

👉 **Observation**: You pulled an image from Docker Hub and served a web page.

---

## 5️⃣ Working with Containers and Docker Hub

### 🛠 Hands-On

```bash
# Run Ubuntu container interactively
docker run -it ubuntu bash

# Inside, create a file
echo "Hello Docker!" > /hello.txt
cat /hello.txt
exit

# List all containers
docker ps -a

# Commit container as new image
docker commit <container_id> myubuntu:1.0

# Tag image with your Docker Hub username
docker tag myubuntu:1.0 <username>/myubuntu:1.0

# Log in to Docker Hub
docker login

# Push image to Docker Hub
docker push <username>/myubuntu:1.0

# Pull image back (to verify)
docker pull <username>/myubuntu:1.0
```

👉 **Observation**: You built your own image and shared it on Docker Hub.

---

## 6️⃣ Working with Dockerfile

Dockerfile automates image creation.

### 🛠 Hands-On

```bash
# Create a project folder
mkdir myapp && cd myapp

# Create Dockerfile
cat <<EOF > Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
EOF

# Create custom web page
echo "<h1>Hello from MyApp Dockerfile!</h1>" > index.html

# Build image with a tag
docker build -t mynginx:v1 .

# List images
docker images

# Run container from custom image
docker run -d --name customnginx -p 8081:80 mynginx:v1

# List running containers
docker ps

# Test
curl http://localhost:8081

# Stop and remove container
docker stop customnginx
docker rm customnginx
```

👉 **Observation**: You customized Nginx with your own page using a Dockerfile.

---

## 7️⃣ Essential Docker Commands Cheat Sheet

```bash
# List all images
docker images

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Run container (detached mode, map ports)
docker run -d -p <host_port>:<container_port> <image_name>

# Attach to running container
docker exec -it <container_id_or_name> bash

# Stop container
docker stop <container_id_or_name>

# Remove container
docker rm <container_id_or_name>

# Remove image
docker rmi <image_id_or_name>

# Show logs from a container
docker logs <container_id_or_name>

# Inspect details about container
docker inspect <container_id_or_name>

# Remove unused data (clean up)
docker system prune -f
```

---

## ✅ Summary

By completing this lab, you have:

* Understood **why containers** exist
* Compared **VMs vs Containers**
* Explored **Docker architecture**
* Pulled & ran images from **Docker Hub**
* Managed containers (start, stop, logs, exec, remove)
* Created & shared your own **custom image**
* Built a **Dockerfile-based app**

```
