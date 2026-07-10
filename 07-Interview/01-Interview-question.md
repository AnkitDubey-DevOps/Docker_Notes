**Q1) What is docker system prune?**
It removes unused Docker data like stopped containers, dangling images, and unused networks.

**Q2) What happens to data in the writable layer if the container is removed?**
That data is lost unless it was stored in a volume or bind mount.

**Q3) What is the writable container layer?**
It is the top layer where changes made by the running container are stored.

**Q4) How do you manage secrets in Docker?**
Use secret managers, environment injection from secure systems, or orchestration tools. Do not hardcode secrets in Dockerfiles.

**Q5)Why should containers avoid running as root?**
Running as root increases security risk if the container is compromised.

**Q6) How can you improve Docker security?**
Use trusted images, run as non-root, keep images small, scan for vulnerabilities, and avoid storing secrets in images.

**Q7) What does unless-stopped mean?**
It restarts the container automatically unless you stopped it manually.

**Q8) What is restart policy in Docker?**
Restart policy tells Docker when to restart containers automatically, like after failure or reboot.

**Q9) What is a health check and why useful in Docker?**
A health check tests whether the app inside the container is working properly.They help detect broken containers even if the process is still running.

**Q10) What is the difference between build-time and run-time configuration?**
Build-time config is used while creating the image.
Run-time config is used when starting the container.
