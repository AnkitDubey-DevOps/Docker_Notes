## Docker Image Hardening
it is the process of securing a container image by minimizing its components, fixing vulnerability, and ensuring least privilege configuration to reduce ut overall attack surface.

## Manual Dockerfile hardening Best Practice 

you can manually harden any image by writing optimized Dockerfile using these strategies.

* **Use Minimal Base Images:** Avoid full OS images like `ubuntu`. Opt for minimal distributions like `alpine` or "distroless" images.
* **Implement Multi-Stage Builds:** Compile your code in a heavyweight build stage. Copy only the final executable into a clean, minimal runtime stage.
* **Run as a Non-Root User:** Containers run as `root` by default. Explicitly declare a `USER` block to drop privileges.
* **Pin Specific Image Tags:** Never use `FROM image:latest`. Pin exact versions or image digests (e.g., `image:1.2.3@sha256:...`) to ensure reproducible, predictable builds.
* **Strip Unnecessary Tools:** Remove compilers, debugging utilities, and package managers from your production container.
* **Clean Package Caches:** If you must install packages via `apt` or `apk`, clean the temporary caches in the same `RUN` command layer to minimize image size and hidden footprint.
* **Do Not Hardcode Secrets:** Never bake API keys, SSH credentials, or passwords into your image layers. Inject them at runtime using environment variables or volume mounts.

An example of a hardened Dockerfile for a Node application:
