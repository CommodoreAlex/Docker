# Docker Basics

This section covers the essential Docker commands and concepts to help you get started with managing containers, images, and volumes.

## Key Docker Commands
Below are some fundamental Docker commands you should know:

### 1. **Working with Images**
- **Search for Images:**
  ```bash
  docker search <image-name>
  ```
  Example:
  ```bash
  docker search nginx
  ```

- **Pull an Image:**
  ```bash
  docker pull <image-name>
  ```
  Example:
  ```bash
  docker pull ubuntu
  ```

- **List Images:**
  ```bash
  docker images
  ```

### 2. **Working with Containers**
- **Run a Container:**
  ```bash
  docker run <image-name>
  ```
  Example:
  ```bash
  docker run ubuntu
  ```

- **Run a Container with Interactive Shell:**
  ```bash
  docker run -it <image-name> /bin/bash
  ```

- **List Running Containers:**
  ```bash
  docker ps
  ```

- **List All Containers (including stopped):**
  ```bash
  docker ps -a
  ```

- **Stop a Running Container:**
  ```bash
  docker stop <container-id>
  ```

- **Remove a Container:**
  ```bash
  docker rm <container-id>
  ```

### 3. **Removing Images**
- **Remove an Image:**
  ```bash
  docker rmi <image-id>
  ```

## Basic Docker Concepts

### 1. **Images**
- Docker images are lightweight, standalone, and executable packages that include everything needed to run an application: code, runtime, libraries, and dependencies.
- You can think of images as the "blueprints" for containers.

### 2. **Containers**
- Containers are running instances of Docker images. They are isolated and include all the resources and dependencies needed to execute an application.

### 3. **Volumes**

### **What Are Docker Volumes?**

- **Definition:** Volumes are directories or files outside a container's writable layer that exist on the host filesystem. They are designed to store data and be easily managed by Docker.
- **Purpose:** Containers are ephemeral, meaning their data is lost when they are stopped or removed. Volumes help solve this problem by providing a persistent storage solution.

### **Why Use Volumes?**

1. **Data Persistence**
    - Containers are stateless by default, so any data stored inside them is temporary. Volumes ensure data survives even if a container is removed or updated.
2. **Sharing Data Between Containers**
    - Multiple containers can access the same data via a shared volume, enabling seamless data exchange.
3. **Performance**
    - Volumes often provide better performance than writing data to the container's writable layer.
4. **Backup and Restore**
    - Volumes can be backed up and restored independently of containers.

### **Types of Docker Volumes**

1. **Named Volumes**
    
    - Managed entirely by Docker. You specify the volume name, and Docker handles the storage location.
    - Example: `docker volume create myvolume`
2. **Anonymous Volumes**
    
    - Created automatically without a specific name. They are harder to track because they have no identifier.
    - Example: `docker run -v /data busybox`
3. **Bind Mounts**
    
    - Maps a directory on the host to a directory in the container. Useful for development and debugging.
    - Example: `docker run -v /host/path:/container/path busybox`

#### Key Commands for Volumes:
- **Create a Volume:**
  ```bash
  docker volume create <volume-name>
  ```

- **List Volumes:**
  ```bash
  docker volume ls

# Example of listing volumes
DRIVER    VOLUME NAME
local     prometheus-setup_prom_data
  ```

- **Inspect a Volume:**
  ```bash
  docker volume inspect <volume-name>

# Example of inspecting a volume
[
    {
        "CreatedAt": "2025-01-22T19:35:44-05:00",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "prometheus-setup",
            "com.docker.compose.version": "1.29.2",
            "com.docker.compose.volume": "prom_data"
        },
        "Mountpoint": "/var/lib/docker/volumes/prometheus-setup_prom_data/_data",
        "Name": "prometheus-setup_prom_data",
        "Options": null,
        "Scope": "local"
    }
]
  ```

- **Remove a Volume:**
  ```bash
  docker volume rm <volume-name>
  ```

---

Practice these commands to build confidence in using Docker. Once comfortable, move on to the next section to learn about building custom Docker images.
