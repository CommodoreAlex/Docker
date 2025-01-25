# Common Docker Issues Troubleshooting Guide 🛠️

This guide provides solutions to some of the most common Docker issues you may encounter. Follow these steps to troubleshoot your Docker containers, images, and services effectively.

## 1. Docker Daemon Not Starting ❌

**Issue**: The Docker daemon isn't running, and you can't run Docker commands.

**Solution**:

- Check Docker status:  
```bash
sudo systemctl status docker
```


- Start the Docker service:
```bash
sudo systemctl start docker
```

- If the issue persists, check Docker logs for errors:
```bash
sudo journalctl -u docker.service
```

## 2. Docker Container Fails to Start 🚫

**Issue**: Your container fails to start after running `docker run`.

**Solution**:

- Check the container logs for errors:
```bash
docker logs <container-id>
```

- Inspect the container status:
```bash
docker ps -a
```

Verify image compatibility (e.g., correct architecture, missing dependencies).

## 3. "Port Already in Use" Error ⚠️

**Issue**: Docker reports that a port is already in use, but you’re unsure which process is using it.

**Solution**:

- Find the process using the port:
```bash
sudo lsof -i :<port-number>
```

* You can also look for internally listening ports to find what you're looking for:
```bash
ss -ntplu
```

- Kill the process:
```bash
sudo kill <pid>  
```

Also make sure nothing is blocked inbound or outbound (based on organizational requirements) for the port in question.
## 4. Insufficient Disk Space 🏴

**Issue**: Docker container or image creation fails due to insufficient disk space.

**Solution**:

- Check disk space usage:
```bash
df -h
```

- Remove unused containers, images, and volumes:
```bash
docker system prune -a
```

- Clean up dangling volumes:
```bash
docker volume prune
```

## 5. Network Issues Between Containers 🌐

**Issue**: Containers cannot communicate with each other.

**Solution**:

 Ensure containers are connected to the correct network.
 
- Check network settings:
```bash
docker network ls
```

- Inspect the network configuration:
```bash
docker network inspect <network-name>
```

## 6. Permission Denied Errors 🔒

**Issue**: You receive "Permission Denied" when running Docker commands or accessing volumes.

**Solution**:

- Add your user to the Docker group:
```bash
sudo usermod -aG docker $USER
```

- Restart your session or log out and back in.
- Check permissions on mounted volumes and files.

## 7. Docker Image Build Fails 🚧

**Issue**: Docker image build fails due to errors in the `Dockerfile`.

**Solution**:

- Review the error message for missing dependencies or incorrect commands.
- Use the `--no-cache` flag to build without cache:
```bash
docker build --no-cache -t <image-name> .
```

Check the context (files in the build directory) for necessary files.

## 8. Docker Compose Not Working 🧩

**Issue**: Docker Compose isn't starting or having issues with services.

**Solution**:

Ensure the `docker-compose.yml` file is correctly formatted.

- Check the logs for detailed error messages:
```bash
docker-compose logs
```

- Try rebuilding services:
```bash
docker-compose up --build
```

---

If none of these solutions fix your issue, refer to Docker’s official documentation and community forums for further assistance: [Docker Desktop Troubleshooting Guide](https://docs.docker.com/desktop/troubleshoot-and-support/troubleshoot/)

It's also helpful to search for error logs or messages online as others may have faced similar issues.

