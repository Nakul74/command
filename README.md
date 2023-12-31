# ---------------------------Docker commands

## List images

```bash
docker images
```
</br>

## List running containers

```bash
docker ps
```
</br>

## List all containers (running + stopped)

```bash
docker ps -a
```
</br>

## Build image

```bash
docker build -t image_name .
```
</br>

## Build container (remove -d if not want to run container in detach mode)

```bash
docker run --name container_name -d -p host_port:container_port image_name
```
</br>

## Build container with volume logs map

```bash
docker run --name container_name -d -p host_port:container_port -v $(pwd)/logs:/app/logs image_name
```
</br>

## Build container with host network

```bash
docker run --name container_name --network="host" -d -p host_port:container_port image_name
```
</br>

## Remove untagged (None) or unused images

##### Remove all unused images (including "none" tagged)
```bash
docker images -f "dangling=true" -q | xargs -r docker rmi -f
```
##### Remove only "none" tagged images
```bash
docker images -a | grep none | awk '{ print $3; }' | xargs docker rmi
```
</br>

## Remove all images

```bash
docker rmi $(docker images -q)
```
</br>

## Remove all containers

```bash
docker rm -vf $(docker ps -a -q)
```
</br>

## Save Docker images as tar

```bash
docker save -o file.tar image_name
```
</br>

## Load Docker image from .tar

```bash
docker load -i file.tar
```
</br>

## Inside Docker container

```bash
docker exec -it container_id /bin/bash
```
</br>

## Docker logs

```bash
docker logs container_id
```
</br>

## Build Docker Compose

```bash
docker compose up
```
or
```bash
rm -rf __docker_compose_logs__ && mkdir __docker_compose_logs__ && nohup docker compose up --remove-orphans --build >> __docker_compose_logs__/out 2>> __docker_compose_logs__/error &
```
</br>

## Down Docker Compose

```bash
docker compose down
```
</br>

## Clean Docker cache

```bash
docker builder prune -a --filter until=24h
```
</br>
</br>
</br>


# ---------------------------Linux commands

## Uvicorn run command

```bash
uvicorn main:app --host 0.0.0.0 --port 1020 --reload
```
or
```bash
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup uvicorn main:app --host 0.0.0.0 --port 8000 --reload >> __public_logs__/out 2>> __public_logs__/error & echo $! > task_id.txt
```
</br>

## Streamlit command

```bash
streamlit run app.py --server.port 8115
```
or
```bash
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup streamlit run app.py --server.port 8115 >> __public_logs__/out 2>> __public_logs__/error & echo $! > task_id.txt
```
</br>

## Nohup command examples

##### Without task_id.txt
```bash
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup python -u app.py >> __public_logs__/out 2>> __public_logs__/error &
```
##### With task_id.txt
```bash
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup python -u app.py >> __public_logs__/out 2>> __public_logs__/error & echo $! > task_id.txt
```
</br>

## Kill nohup process

```bash
kill -9 `cat task_id.txt`
```
</br>

## Get used port details

```bash
sudo netstat -ltup | grep 8111
```
or
```bash
sudo netstat -nlp | grep :8111
```
</br>

## Kill port

```bash
sudo kill -9 $(lsof -i:8000 -t)
```
</br>

## Get host IP address

```bash
curl icanhazip.com
```
</br>

## Nginx update conf

```bash
sudo echo "server {
    listen 8014;
    server_name $(curl icanhazip.com);
    location / {
        proxy_pass http://127.0.0.1:8015;
    }
}" > /etc/nginx/sites-enabled/fastapi_nginx
```
</br>

## Celery run command

```bash
celery -A file_name worker --loglevel=info
```
or
```bash
celery -A file_name worker --loglevel=info --logfile=log_dir/celery.log
```
or
```bash
nohup celery -A file_name worker --loglevel=info >> log_dir/out 2>> log_dir/error & echo $! > task_id.txt
```

### Flower run command
```bash
celery -A file_name flower --port=5001
```
