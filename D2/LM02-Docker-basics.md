---

# Task 11 – Run Your First Container

```bash
sudo docker run hello-world
```

Expected

```text
Hello from Docker!
```

---

# Task 12 – Search Docker Hub

```bash
docker search nginx
```

---

# Task 13 – Download an Image

```bash
docker pull nginx
```

Verify

```bash
docker images
```

Expected

```text
REPOSITORY    TAG
nginx         latest
```

---

# Task 14 – Run Nginx Container

```bash
docker run -d \
--name webserver \
-p 9080:80 \
nginx
```

Verify

```bash
docker ps
```

---

# Task 15 – Test Nginx

Check locally.

```bash
curl localhost:9080
```

Or open

```text
http://SERVER-IP:8080
```

You should see the **Welcome to nginx!** page.

---

# Task 16 – View Running Containers

```bash
docker ps
```

Show all containers.

```bash
docker ps -a
```

---

# Task 17 – Inspect Container

```bash
docker inspect webserver
```

---

# Task 18 – View Container Logs

```bash
docker logs webserver
```

---

# Task 19 – Execute Commands Inside Container

```bash
docker exec -it webserver bash
```

Check files.

```bash
ls
```

Exit.

```bash
exit
```

---

# Task 20 – Stop and Start Container

Stop

```bash
docker stop webserver
```

Start

```bash
docker start webserver
```

Restart

```bash
docker restart webserver
```

---

# Task 21 – Remove Container

Stop container.

```bash
docker stop webserver
```

Delete container.

```bash
docker rm webserver
```

---

# Task 22 – Remove Image

```bash
docker rmi nginx
```

List images.

```bash
docker images
```

---

# Task 23 – Run Docker Without sudo (Optional)

Add current user to Docker group.

```bash
sudo usermod -aG docker $USER
```

Apply changes.

```bash
newgrp docker
```

Verify.

```bash
docker ps
```

---

# Useful Docker Commands

| Command                            | Description                     |
| ---------------------------------- | ------------------------------- |
| `docker --version`                 | Display Docker version          |
| `docker info`                      | Show Docker system information  |
| `docker images`                    | List downloaded images          |
| `docker ps`                        | List running containers         |
| `docker ps -a`                     | List all containers             |
| `docker pull <image>`              | Download an image               |
| `docker run <image>`               | Create and run a container      |
| `docker stop <container>`          | Stop a container                |
| `docker start <container>`         | Start a stopped container       |
| `docker restart <container>`       | Restart a container             |
| `docker exec -it <container> bash` | Open a shell inside a container |
| `docker logs <container>`          | View container logs             |
| `docker rm <container>`            | Remove a container              |
| `docker rmi <image>`               | Remove an image                 |

---

# Verification Checklist

| Task                                      | Status |
| ----------------------------------------- | ------ |
| Ubuntu Updated                            | ☐      |
| Docker Repository Added                   | ☐      |
| Docker Installed                          | ☐      |
| Docker Service Running                    | ☐      |
| Hello-World Container Executed            | ☐      |
| Nginx Image Downloaded                    | ☐      |
| Nginx Container Running                   | ☐      |
| Nginx Accessible on Port 8080             | ☐      |
| Docker Without sudo Configured (Optional) | ☐      |

---

# Troubleshooting

| Problem                            | Solution                                                                                       |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- |
| `docker: command not found`        | Ensure Docker packages are installed and open a new shell session.                             |
| Permission denied to Docker socket | Add the user to the `docker` group and run `newgrp docker` or log out and back in.             |
| Docker service not running         | Start it with `sudo systemctl start docker` and enable it with `sudo systemctl enable docker`. |
| Unable to pull images              | Verify internet connectivity and DNS resolution (`ping download.docker.com`).                  |
| Port 8080 already in use           | Run the container with a different host port, e.g., `-p 8081:80`.                              |

---

# Lab Summary

In this lab, you successfully:

* Installed Docker Engine on Ubuntu 24.04 LTS.
* Configured the official Docker APT repository.
* Verified the Docker service and installation.
* Pulled images from Docker Hub.
* Ran, managed, and removed Docker containers.
* Deployed an Nginx web server in a container and accessed it through a web browser.

This lab forms the foundation for subsequent exercises involving Dockerfiles, Docker Compose, container networking, volumes, and multi-container applications.
