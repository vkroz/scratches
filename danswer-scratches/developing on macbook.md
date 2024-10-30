For full documentation see: https://docs.danswer.dev/introduction

# Developing on Macbook

- Forked Repo: https://github.com/vkroz/danswer
- Local copy: $HOME/workspace/danswer/
- Deployment configs: https://github.com/vkroz/danswer/tree/main/deployment

## Debugging application

### Backend REST API

Stop docker container if running:
```
CONTAINER_NAME="danswer-stack-web_server-1"
docker stop $(docker ps --filter "name=^/${CONTAINER_NAME}$" --filter "status=running" -q)
```

Run backend REST API on localhost:
```
alembic upgrade head
uvicorn danswer.main:app --host 0.0.0.0 --port 8080"
```


### Frontend

Stop docker container if running:
```
CONTAINER_NAME="danswer-stack-web_server-1"
docker stop $(docker ps --filter "name=^/${CONTAINER_NAME}$" --filter "status=running" -q)
```

Run front-end on localhost:
```
cd web
npm run dev
```




## Running using docker compose
Instruction https://github.com/danswer-ai/danswer/tree/main/deployment/docker_compose

### Launch application
There are options to package and launch with docker compose

**Rebuild and start:**
```
cd deployment/docker_compose
docker compose -f docker-compose.dev.yml -p danswer-stack up -d --build --force-recreate
```

**Resume stopped containers exactly as they were:**
```
docker compose -f docker-compose.dev.yml -p danswer-stack up -d
```
or 
```
docker compose -f docker-compose.dev.yml -p danswer-stack start
```

> Both commands will resume the stopped containers, but using `-d`  will additionally (1) create any missing containers, (2) Update container configurations if the compose file changed, and (3) create missing networks


### Stop 

**Stop (just pause) all containers:**

```
docker compose stop
```

**Stop danswer stack:**
```
docker compose -f docker-compose.dev.yml -p danswer-stack stop
```


**Stop individual container**
```
docker stop container_name
```

**Stop all running containers**
```
docker stop $(docker ps -q)
```

### Stop and destroy

**Stop and destroy everything:**
```
docker compose down
```
If you need to stop and remove everything (containers, networks, volumes):
```
docker compose down --volumes --remove-orphans
```

**Stop and destroy only danswer stack**
```
docker compose -f docker-compose.dev.yml -p danswer-stack down
```

If you also want to remove volumes with down, you need to add `--volumes` flag:
```
docker compose -f docker-compose.dev.yml -p danswer-stack down --volumes
```
