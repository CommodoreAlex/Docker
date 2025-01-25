# Building Custom Docker Images 🛠️

This section covers the steps to build your own custom Docker images, starting from writing a Docker file to building and testing your image.

## Key Concepts

### 1. **Docker file**
A Docker file is a text file that contains a series of instructions to build a Docker image. It defines the base image, the software environment, and the application setup that you want to include in the image.

### 2. **Base Image**
The base image is the starting point for your custom image. It provides the operating system and libraries that your application will run on. Common base images include `ubuntu`, `alpine`, or `node`.

---

## Steps to Build a Custom Image

### 1. **Create a Docker file**

First, you'll need to create a Docker file. Here's an example:

```Dockerfile
# Use an official base image
FROM ubuntu:20.04

# Set environment variables (optional)
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary dependencies
RUN apt-get update && \
    apt-get install -y python3 python3-pip

# Set working directory
WORKDIR /app

# Copy the application code into the container
COPY . /app

# Install Python dependencies (if any)
RUN pip3 install -r requirements.txt

# Expose the port your application will run on
EXPOSE 5000

# Define the command to run your app
CMD ["python3", "app.py"]
```

Let's break down all of the key elements in this Docker File example.

### 1. **requirements.txt**

In Python projects, `requirements.txt` is a file that lists all the external dependencies (libraries) that your application needs to run. It's commonly used in Python projects to manage dependencies in a simple and reproducible way.

#### How it works:

- You define the dependencies in the `requirements.txt` file, typically by specifying package names and their versions.
- Docker will use this file to install the necessary dependencies when building your image.

Example of a `requirements.txt` file:
```txt
flask==2.0.1
requests==2.26.0
pandas==1.3.2
```

These are Python libraries that your app will use. In this case:

- **Flask** is a lightweight web framework.
- **Requests** is a library for making HTTP requests.
- **Pandas** is used for data manipulation and analysis.

In your Docker file, you typically use the following command to install these dependencies:
```dockerfile
RUN pip3 install -r requirements.txt
```

When Docker runs this command, it installs all the Python packages listed in `requirements.txt` inside the container.

### 2. **Elements of the Docker file**

Now, let's dive deeper into the other key elements of the Dockerfile.

#### `FROM`

The `FROM` instruction specifies the base image for the Docker image you are building. The base image contains the operating system and necessary components.

Example:
```dockerfile
FROM ubuntu:20.04
```

This tells Docker to use the official Ubuntu 20.04 image as the base for your custom image. You can specify different base images depending on your application requirements (e.g., `python:3.8`, `node:14`, etc.).

#### `ENV`

The `ENV` instruction sets environment variables inside the container.

Example:
```dockerfile
ENV DEBIAN_FRONTEND=noninteractive
```

This is often used to prevent interactive prompts during package installations (like `apt-get` on Debian/Ubuntu), making the build process more efficient.

#### `RUN`

The `RUN` instruction allows you to execute commands inside the container while building the image. It is often used to install packages, update system files, or run shell commands.

Example:
```dockerfile
RUN apt-get update && \
    apt-get install -y python3 python3-pip
```

In this case, we're updating the package index (`apt-get update`) and installing Python 3 and `pip` (Python's package manager). The `&&` combines multiple commands into one layer, and the backslash (`\`) is used to break the command into multiple lines for readability.

#### `WORKDIR`

The `WORKDIR` instruction sets the working directory inside the container for any subsequent instructions (like `RUN`, `COPY`, `CMD`, etc.). If the directory doesn’t exist, it will be created.

Example:
```dockerfile
WORKDIR /app
```

This sets the working directory to `/app`. Any subsequent commands like `COPY . /app` or `RUN` will use `/app` as the current directory.

#### `COPY`

The `COPY` instruction is used to copy files and directories from the host machine into the container. You can specify the source path on the host and the destination path inside the container.

Example:
```dockerfile
COPY . /app
```

This copies everything from the current directory (`.`) on the host machine into the `/app` directory inside the container.

#### `EXPOSE`

The `EXPOSE` instruction informs Docker that the container will listen on specific network ports at runtime. This is a way to document which ports your app uses, although it doesn’t actually publish the port.

Example:
```dockerfile
EXPOSE 5000
```

`EXPOSE 5000`

This indicates that your container will expose port `5000` (e.g., if you're running a web application on that port).

#### `CMD`

The `CMD` instruction sets the default command to run when a container starts. It’s typically used to run the main application in your container.

Example:
```dockerfile
CMD ["python3", "app.py"]
```

This will run `python3 app.py` when the container starts. If you want to override the `CMD`, you can provide a different command when running the container with `docker run`.

---

### Summary of Common Docker file Instructions:

- **FROM**: Specifies the base image.
- **ENV**: Sets environment variables inside the container.
- **RUN**: Executes commands inside the container (e.g., installing dependencies).
- **WORKDIR**: Sets the working directory inside the container.
- **COPY**: Copies files from the host machine to the container.
- **EXPOSE**: Documents the ports the container will use.
- **CMD**: Defines the command that runs when the container starts.

---

### 2. **Build the Image**

The Docker file should simply be named `Dockerfile` (with no file extension). By default, Docker looks for a file named `Dockerfile` when you run the `docker build` command.

Once your Docker file is ready, you can build your image with the `docker build` command:
```bash
docker build -t my-custom-image .
```

This will build the image using the current directory (`.`) and tag it as `my-custom-image`.

If you want to use a custom name for your Docker file, you can specify it with the `-f` flag when building the image:
```bash
docker build -f MyDockerfile -t my-custom-image .
```

But typically, for simplicity and convention, it's best to stick with just `Dockerfile`.
### 3. **Check the Built Image**

You can check if your custom image is built successfully by listing all available images:
```bash
docker images
```

Your image should appear in the list.

### 4. **Run a Container from the Custom Image**

Now that you have your custom image, you can run a container using the `docker run` command:
```bash
docker run -d -p 5000:5000 my-custom-image
```

This will start your container in detached mode and expose port 5000 on the host machine.

Detached mode runs a Docker container in the background, so it doesn't block your terminal. You can keep working in your terminal while the container continues running.

To view its logs or stop it, you can use `docker logs` and `docker stop`.

### 5. **Test the Container**

After running the container, open a web browser and navigate to `http://localhost:5000` (or whatever port you exposed). If everything is set up correctly, you should see your application running inside the container!

---

## Advanced Docker file Instructions

### 1. **Multi-Stage Builds**

**Multi-stage builds** allow you to create smaller and more efficient Docker images by separating the build environment from the production environment. In a typical Docker file, all steps are executed in one layer, but with multi-stage builds, you can have multiple `FROM` statements to define different stages of the build.

#### Components of Multi-Stage Builds:

- **Build Stage**: This is where you install dependencies, build your application, and prepare it for the production stage. It typically uses a larger image to provide all the tools needed to build the app (e.g., compilers, build tools).
- **Production Stage**: This is the final image, where you only include the necessary components to run the app. It’s usually based on a smaller, lightweight image (e.g., `alpine` or `node:alpine`).

Example:
```dockerfile
# Stage 1: Build environment
FROM node:14 AS build-stage
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Stage 2: Production environment
FROM node:14-slim
WORKDIR /app
COPY --from=build-stage /app/build /app
CMD ["node", "index.js"]

```

#### How it works:

- In **Stage 1**, we use a full `node:14` image to install dependencies and build the app.
- In **Stage 2**, we use a smaller `node:14-slim` image and copy only the necessary build files from Stage 1 (`--from=build-stage`), which reduces the size of the final image.

#### Use Cases for Multi-Stage Builds:

- **Smaller Image Sizes**: By copying only the necessary files to the production stage, you avoid unnecessary tools or build dependencies that are only needed for development.
- **Security**: You minimize the attack surface of your production image by including only the runtime and app files.
- **Cleaner Separation**: It helps you separate the build process from the runtime environment, improving maintainability and readability of your Docker file.

### 2. **Using Docker Compose**

**Docker Compose** is a tool for defining and managing multi-container Docker applications. It allows you to define multiple services (containers) that work together, such as a web server and a database, in a single configuration file (`docker-compose.yml`).

#### Components of Docker Compose:

- **services**: These are the individual containers that make up your application. Each service can define its own image, environment variables, ports, volumes, etc.
- **volumes**: Volumes are used for persistent data storage. They allow data to persist even when containers are stopped or removed.
- **networks**: Defines the network settings for the containers, enabling them to communicate with each other.
- **build**: Instead of using pre-built images, you can use `docker-compose.yml` to define how to build images from a Docker file.

#### Example of Docker Compose Configuration:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - ./app:/app
    environment:
      - DEBUG=true
    networks:
      - frontend
      - backend

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - backend

volumes:
  db_data:

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge

```

#### Explanation of Components:

- **`services`**: There are two services defined here:
    - **web**: This is the main app container. It is built from the current directory (`.`), exposes port 5000, and mounts the local `./app` directory to `/app` inside the container. It is connected to both the `frontend` and `backend` networks, enabling it to communicate with other services that are on these networks.
    - **db**: This is a PostgreSQL database container. It uses the official `postgres` image and defines environment variables for the database user and password. It is connected to the `backend` network, allowing the `web` service to interact with it securely.
- **`volumes`**: The `db_data` volume ensures that the database data persists across container restarts. It is specifically used by the `db` service to store the PostgreSQL data outside of the container’s lifecycle.
- **`networks`**:
    - **`frontend`**: The `web` service is connected to the `frontend` network, enabling communication with any other frontend services (e.g., a load balancer or reverse proxy).
    - **`backend`**: Both the `web` and `db` services are connected to the `backend` network, ensuring that they can communicate with each other directly (e.g., the `web` service can access the `db` service using the service name `db`). This network is isolated from the `frontend` network for security.

#### Use Cases for Docker Compose:

- **Multi-container Applications**: When your app relies on multiple containers (e.g., a web server, a database, and a caching service), Docker Compose helps manage them as a single application.
- **Easier Management**: You can start all the services with a single command (`docker-compose up`), instead of managing each container separately.
- **Development and Testing**: Docker Compose is perfect for local development and testing environments, where you can spin up multiple services (e.g., app, database, caching) that work together.
- **Defining Network and Volumes**: Compose automatically sets up a network for communication between containers and can define persistent storage volumes.

---

### **When to Use Multi-Stage Builds vs. Docker Compose**

- **Use Multi-Stage Builds** when you need to optimize image sizes and improve security by reducing the contents of your production images. This is especially useful for applications that require a complex build process (e.g., compiling code or installing dependencies) and you want to keep the final image clean and small.
- **Use Docker Compose** when you have multiple containers that need to work together, like a web server and a database, and you want a simple way to define and manage them. It is invaluable when running development or testing environments with multiple services that need to be orchestrated.

---

### Summary:

- **Multi-Stage Builds**: Split the Docker file into stages to optimize image sizes, reduce the attack surface, and separate the build and runtime environments.
- **Docker Compose**: A tool to manage multi-container applications by defining services, networks, and volumes in a single configuration file, simplifying the management of complex applications with multiple containers.

Both tools can significantly improve your Docker workflow, but they serve different purposes depending on your needs for efficiency, security, and orchestration.

