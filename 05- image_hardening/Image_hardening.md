## Docker Image Hardening
Container image hardening is a process of securing a container by reducing its attack surface and making it less vulnerable to exploits.

### 1. Multi-Stage Builds
Best Practice: Use multi-stage builds to separate the build environment from the runtime environment. This helps in including only the necessary artifacts in the final image.

Example of Dockerfile:
```
# Build stage
FROM node:14 AS build
WORKDIR /app
COPY . .
RUN npm install

# Runtime stage
FROM alpine:3.14
COPY --from=build /app /app
WORKDIR /app
CMD ["./app"]
```
This Dockerfile is an example of a multi-stage build. The first stage builds the application in a Node.js environment, and the second stage creates a smaller, lightweight runtime image containing only the built application and its runtime dependencies. The multi-stage approach is used to reduce the final image size, which is beneficial for storage, distribution, and security.

### 2. Use Minimal Base Images

Best Practice: Start with a minimal base image like Alpine or a slim version of popular distributions. Smaller images contain fewer components, reducing the potential attack surface.
Example:
```
FROM alpine:3.14
```
### 3. Keep Your Images Up-to-date
Best Practice: Regularly update your images to include the latest security patches. This can be automated using CI/CD pipelines.

Example: In your CI/CD script:
```
docker build --pull -t myimage .
```

###  4. Add the HEALTHCHECK Instruction to the Container Image

Add this instruction to Dockerfiles, and based on the result of the healthcheck (unhealthy), Docker could exit a non-working container and instantiate a new one.

Add HEALTHCHECK to monitor container health :

```
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 CMD curl -f http://localhost:8080/health || exit

```
Then if you run the container using Docker you can use this command to make sure that your unhealty container is restarted :

```
docker run --restart on-failure [other options] [image name]
```

### 5. Specify Non-Root User

Best Practice: Run the container as a non-root user.

### 6. Avoid Leaking Sensitive Information

Best Practice: Never hardcode sensitive information like passwords or API keys (secrets) in the code of your application or in your Dockerfile.

When your application needs to retrieve these secrets:

The recommended approach is to set environment variables when you run a container using docker :

```
docker run -e MY_VARIABLE=my_value my-image
```

### 7. Utilize .dockerignore File

### 8. Scan you Docker image for Vulnerabilities

Best Practice: Regularly scan your container images for vulnerabilities using tools like Trivy, Clair, or Docker’s own scanning feature.

Example: Scanning Images with Trivy
```
# Install Trivy
apt-get install trivy
# Scan a container image for vulnerabilities
trivy image [YOUR_IMAGE_NAME]
```

### 9. Scan you application code for Vulnerabilities and backdoors

Best Practice: Regularly scan your application code with a scanner that does static code analysis.

Example: Scanning Images with SonarQube
```
# Navigate to your project directory
cd /path/to/your/project

# Run SonarScanner analysis
sonar-scanner
```
This scans the code of your application for vulnerabilities, aiding in proactive risk mitigation.

### 10. Minimize Exposed Ports
Best Practice: Only expose the ports that are absolutely necessary for your Docker container. Exposing unnecessary ports increases the attack surface for potential security vulnerabilities. For instance, if a service running on an exposed port has a security flaw, it can be exploited by attackers.


