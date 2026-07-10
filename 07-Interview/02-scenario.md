**Q1) 4. Your app inside the container is not reachable from the browser. What would you do?**

I would check:

1. if the app is running
2. if the correct port is exposed
3. if port mapping is set with -p
4. if the app is listening on 0.0.0.0

**Q2) A database container loses data after restart. How would you fix it?**

Ans: I would use a Docker volume so the data is stored outside the container.

**Q3) You need to run app, database, and Redis together. What would you use?**

Ans: I would use Docker Compose because it manages multi-container applications easily.

**Q4) Two containers need to talk to each other. How would you set it up?**

Ans: I would place them on the same Docker network, preferably with Docker Compose or a custom bridge network.

**Q5) Your build is very slow. How would you improve it?**

Ans: I would optimize Dockerfile layer order, use caching correctly, reduce build context, and avoid copying unnecessary files.
