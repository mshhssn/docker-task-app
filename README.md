# docker-task-app

Small browser-based task app containerized with Docker.

## Purpose

This repository continues the project beyond the earlier lab-specific stage and serves as the working base for further Docker exercises and extensions, including:

- Docker image build and local run
- GitHub Actions for build, test, and publish
- Docker Hub image publishing
- versioned image releases via Git tags
- upcoming Docker Compose setup
- upcoming persistence and volume exercises

## Tech Stack

- HTML
- CSS
- JavaScript
- Nginx (`nginx:alpine`)
- Docker
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
│       └── docker-publish.yml
│       └── greetings.yml
├── Dockerfile
└── README.md
```

## Local Build and Run
1. Build the Docker image:

   ```bash
   docker build -t docker-task-app:latest .
   ```
2. Run the Docker container:

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