# 🐳 Docker Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Docker quick reference.

---

## Images

```bash
docker pull nginx                 # download an image
docker images                     # list local images
docker build -t myapp:1.0 .       # build from Dockerfile
docker rmi nginx                  # remove image
docker tag myapp myapp:latest
docker push user/myapp:1.0        # push to registry
docker image prune                # clean dangling images
```

## Containers

```bash
docker run nginx                          # run (foreground)
docker run -d nginx                       # detached (background)
docker run -d -p 8080:80 nginx            # map host:container port
docker run --name web -d nginx            # named container
docker run -it ubuntu bash                # interactive shell
docker run --rm alpine echo hi            # auto-remove after exit
docker run -e KEY=value myapp             # set env var
docker run -v $(pwd):/app myapp           # bind mount volume

docker ps                                 # running containers
docker ps -a                              # all (incl. stopped)
docker stop web; docker start web; docker restart web
docker rm web                             # remove container
docker rm -f web                          # force remove
```

## Inspect & Debug

```bash
docker logs web                   # view output
docker logs -f web                # follow (live)
docker exec -it web bash          # shell into running container
docker inspect web                # full JSON details
docker stats                      # live resource usage
docker top web                    # processes
docker cp web:/file ./file        # copy out of container
```

## Dockerfile

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

```dockerfile
# Multi-stage build (smaller final image)
FROM node:20 AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```

## Volumes & Networks

```bash
docker volume create data
docker volume ls
docker run -v data:/var/lib/mysql mysql   # named volume

docker network create mynet
docker network ls
docker run --network mynet myapp
```

## Docker Compose

```yaml
# docker-compose.yml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - pgdata:/var/lib/postgresql/data
volumes:
  pgdata:
```

```bash
docker compose up                 # start all services
docker compose up -d              # detached
docker compose up --build         # rebuild images
docker compose down               # stop + remove
docker compose logs -f
docker compose ps
docker compose exec web bash
```

## Cleanup

```bash
docker system prune               # remove unused data
docker system prune -a --volumes  # aggressive cleanup ⚠️
docker container prune
docker volume prune
```

---

[🔝 Back to README](../README.md)
