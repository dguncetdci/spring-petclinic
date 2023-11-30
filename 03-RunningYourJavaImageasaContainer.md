
# Running Your Java Image as a Container

## Prerequisites
- Complete the steps in "Build your Java image."

## Overview
You've built a Docker image using `docker build`. Now, it's time to run it and test your application.

### Running the Image
Use the `docker run` command to start your image:

```bash
docker run java-docker
```

### Testing the Container
Make a GET request to your running server:

```bash
curl --request GET --url http://localhost:8080/actuator/health --header 'content-type: application/json'
```

If it fails to connect, it's due to the container running in isolation, including networking.

### Publishing a Port
Stop the container using `ctrl-c`, then restart it, publishing port 8080:

```bash
docker run --publish 8080:8080 java-docker
```

Rerun the `curl` command to test the connection again.

## Running in Detached Mode
Run your container in detached mode using `-d`:

```bash
docker run -d -p 8080:8080 java-docker
```

### Verifying the Container's Status
Check if the container is running:

```bash
docker ps
```

### Naming Containers
To name your container, use the `--name` flag with `docker run`. For example:

```bash
docker run --rm -d -p 8080:8080 --name springboot-server java-docker
```

## Stopping and Removing Containers
Stop and remove your containers using:

```bash
docker stop [container_name]
docker rm [container_name]
```

With these commands, you can manage and organize your Docker containers effectively.
