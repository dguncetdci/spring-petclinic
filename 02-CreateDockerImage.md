
# Building Your First Docker Image

## Building the Image

```bash
docker build --tag java-docker .
```

This command sends the build context to the Docker daemon. The output should be similar to:

```
Sending build context to Docker daemon  5.632kB
Step 1/7 : FROM eclipse-temurin:17-jdk-jammy
Step 2/7 : WORKDIR /app
...
Successfully built a0bb458aabd0
Successfully tagged java-docker:latest
```

## Viewing Local Images

To see a list of images on your local machine, use the Docker CLI. Run the following command:

```bash
docker images
```

The output should include the image you just built:

```
REPOSITORY          TAG                 IMAGE ID            CREATED          SIZE
java-docker         latest              b1b5f29f74f0        47 minutes ago   567MB
```

## Tagging Images

An image name consists of slash-separated components and may include lowercase letters, digits, and separators (period, underscores, dashes).

### Creating a New Tag

```bash
docker tag java-docker:latest java-docker:v1.0.0
```

This command creates a new tag, `java-docker:v1.0.0`, pointing to the same image.

### Viewing the New Tag

After tagging, you can see the new tag by running:

```bash
docker images
```

```
REPOSITORY    TAG      IMAGE ID        CREATED         SIZE
java-docker   latest   b1b5f29f74f0    59 minutes ago  567MB
java-docker   v1.0.0   b1b5f29f74f0    59 minutes ago  567MB
```

Notice that both tags refer to the same IMAGE ID.

### Removing a Tag

To remove a tag, use the `rmi` command:

```bash
docker rmi java-docker:v1.0.0
```

This command untags `java-docker:v1.0.0`. Verify by running `docker images` again:

```
REPOSITORY      TAG     IMAGE ID        CREATED         SIZE
java-docker     latest  b1b5f29f74f0    59 minutes ago  567MB
```

The tag `:v1.0.0` is removed, but the `java-docker:latest` tag is still available.
