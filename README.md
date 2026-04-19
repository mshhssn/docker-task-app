# docker-task-app

Small browser-based task app containerized with Docker.

## Purpose

This repository continues the project beyond the earlier lab-specific stage and serves as the working base for further Docker exercises and operations-focused extensions, including:

- Docker image build and local run
- GitHub Actions for build, test, and publish
- Docker Hub image publishing
- versioned image releases via Git tags
- Docker Compose-based service operation
- bind mounts and Docker volumes
- upcoming reverse proxy and ingress-related exercises

## Tech Stack

- HTML
- CSS
- JavaScript
- Nginx (`nginx:alpine`)
- Docker
- Docker Compose
- GitHub Actions
- Docker Hub

## Project Structure

```text
.
├── app/
│   ├── index.html
│   ├── style.css
│   └── app.js
├── .github/
│   └── workflows/
│       ├── docker-publish.yml
│       └── greetings.yml
├── .env
├── compose.yml
├── Dockerfile
└── README.md
```

## Local Build and Run
1. Build the Docker image locally:

   ```bash
   docker build -t docker-task-app:latest .
   ```
2. Run the Docker container manually:

   ```bash
   docker run -d -p 8080:80 --name docker-task-app docker-task-app:latest
   ```
3. Verify the container is running:

   ```bash
   docker ps
   ```
4. Test it by sending a request to the app:

   ```bash
   curl http://localhost:8080
   ```
5. Access the app in your browser at `http://localhost:8080`.
6. Stop and remove the container when done:

   ```bash
   docker stop docker-task-app
   docker rm docker-task-app
   ```

## Docker Compose Operations
The Repository now uses Docker Compose for easier multi-container management as the main operational entry point.

1. You can start the app with:

   ```bash
   docker-compose -f compose.yml up -d --build
   ```
2. Verify the container is running:

   ```bash
   docker-compose -f compose.yml ps
   ```
3. Access the app in your browser at `http://localhost:8080`.
4. View logs with:
   ```bash
   docker-compose -f compose.yml logs -f
   ```
   or
   ```bash
   docker compose logs docker-task-app
   ```
   For a specific service use:
   ```bash
   docker-compose -f compose.yml logs -f docker-task-app
   ```
5. Stop the services when done:
   ```bash
   docker-compose -f compose.yml stop
   ```
6. Start the services again without rebuilding:
   ```bash
   docker-compose -f compose.yml start
   ```
7. To rebuild and restart the services:
   ```bash
   docker-compose -f compose.yml up -d --build
   ```
8. To remove the container, services and associated resources:
   ```bash
   docker-compose -f compose.yml down
   ```
9. To remove the container, services and associated resources including volumes:
   ```bash
   docker-compose -f compose.yml down -v
   ```

## Configuration and Environment Variables '.env' file
The `.env` file is used to store environment variables that can be referenced in the `compose.yml` file. This allows you to manage configuration values separately from the code and Docker configuration. For example, you can define the following variables in the `.env` file:

```env
APP_PORT=8080
```
Then, in the `compose.yml` file, you can reference these variables like this:

```yaml
services:
  docker-task-app:
    image: docker-task-app:latest
    ports:
      - "${APP_PORT}:80"
```
This way, you can easily change the port mapping by updating the `.env` file without modifying the `compose.yml` file directly.

The Compose setup uses .env to externalize runtime defaults such as:

   - application port
   - image tag
   - image name
   - container name

## Storage Concepts
### Bind Mounts
The project uses bind mounts to map the local `app/` directory to the container's `/usr/share/nginx/html` directory.
- This allows you to edit the application files locally and see the changes reflected in the running container without needing to rebuild the image.
- The bind mount is defined in the `compose.yml` file under the `volumes` section for the `docker-task-app` service.
- This setup is ideal for development and testing purposes, as it provides a seamless workflow for making changes to the application and immediately seeing the results in the containerized environment.
- For production use, modification to the local files won't persist in the container after the container is stopped and removed, since bind mounts are not designed for persistent storage.

The application content is mounted from the host into the container:

   - host path: ./app
   - container path: /usr/share/nginx/html
   - mounted as read-only

### Docker Volumes
While this project primarily uses bind mounts for development, Docker volumes can be used for persistent storage in production scenarios. Docker volumes are managed by Docker and provide a way to store data outside of the container's filesystem, allowing data to persist even if the container is removed. In this project, you could create a Docker volume for the application data if you wanted to persist changes made within the container, but for development purposes, bind mounts are more convenient for real-time editing.

A Docker-managed volume is mounted for persistent test data:

   - volume name: task-app-data
   - container path: /var/lib/task-app-data

This is used to demonstrate persistence behavior across container removal and recreation.

### Persistence Behavior
Important oeprational difference:
- Bind mounts: Changes to files on the host are immediately reflected in the container, but changes made within the container are not persisted if the container is removed.
- Docker volumes: Data is stored in a location managed by Docker, ensuring it persists even if the container is removed.
- `docker compose down` removes containers and network, but keeps volumes
- `docker compose down -v` also removes the associated volumes

This means data stored in a Docker volume survives `docker compose down`, but not `docker compose down -v`.

## GitHub Actions
The repository includes two GitHub Actions workflows:
- `docker-publish.yml`: Builds and publishes the Docker image to Docker Hub on push to the `main` branch and on Git tag creation.
- `greetings.yml`: A simple workflow that runs on push to the `main` branch and prints a greeting message in the workflow logs.

## CI/CD
The GitHub Actions workflow builds, tests, and publishes the image to Docker Hub.
- On push to `main`, it builds the image and publishes `latest` and `sha-*`.
- On Git tags such as v1.0.0, it publishes a versioned Docker image tag such as 1.0.0.

## Docker Hub Repository
The Docker image is published to Docker Hub under the repository `mshhssn/docker-task-app`. You can pull the image using:

```bash
docker pull mshhssn/docker-task-app:latest
   ```
   or for a specific version:
```bash
docker pull mshhssn/docker-task-app:1.0.0
```

## Notes
- GitHub stores how the image is built, validated and published in the `docker-publish.yml` workflow file. You can customize it further as needed.
- Docker Hub stores the published images and provides versioning through tags.
- You can manage your images and tags through the Docker Hub web interface.
- This Repository is the continued working project based on the earlier lab state.