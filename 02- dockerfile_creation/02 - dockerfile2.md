## 8. CMD vs ENTRYPOINT — What to Run

```Dockerfile
# CMD — default command, easily overridden
CMD ["node", "server.js"]
CMD ["npm", "start"]

# ENTRYPOINT — the main executable, harder to override
ENTRYPOINT ["python", "app.py"]
```


### Best pattern — use both:

```Dockerfile
ENTRYPOINT ["python", "app.py"]   # Fixed: always runs python
CMD ["--port", "8080"]            # Default args, easily overridden
```

---

## 9. VOLUME — Persist Data

```Dockerfile
VOLUME /app/data
VOLUME /var/log/nginx
```

- Marks a directory to be persisted outside the container
- Data survives container restarts/removal

---

## 10. USER — Run as Non-Root

```Dockerfile
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

- By default containers run as root — a security risk
- Always switch to a non-root user before `CMD`

---

## 11. HEALTHCHECK — Monitor Container Health

```Dockerfile
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

- Docker will mark your container as healthy or unhealthy
- Essential for production and orchestration (Kubernetes, ECS)

---

## 12. LABEL — Metadata

```Dockerfile
LABEL maintainer="you@example.com"
LABEL version="1.0"
LABEL description="My awesome app"
```

- Purely informational, doesn't affect the image
