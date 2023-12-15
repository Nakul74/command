# ---------------------------Docker commands

# List images
docker images

# List running containers
docker ps

# List all containers (running + stopped)
docker ps -a

# Build image
docker build -t image_name .

# Build container (remove -d if not want to run container in detach mode)
docker run --name container_name -d -p host_port:container_port image_name

# Build container with volume logs map
docker run --name container_name -d -p host_port:container_port -v $(pwd)/logs:/app/logs image_name

# Build container with host network
docker run --name container_name --network="host" -d -p host_port:container_port image_name

# Remove untagged (None) or unused images
# Remove all unused images (including "none" tagged)
docker images -f "dangling=true" -q | xargs -r docker rmi -f
# Remove only "none" tagged images
docker images -a | grep none | awk '{ print $3; }' | xargs docker rmi

# Remove all images
docker rmi $(docker images -q)

# Remove all containers
docker rm -vf $(docker ps -a -q)

# Save Docker images as tar
docker save -o file.tar image_name

# Load Docker image from .tar
docker load -i file.tar

# Inside Docker container
docker exec -it container_id /bin/bash

# Docker logs
docker logs container_id

# Build Docker Compose
docker compose up
rm -rf __docker_compose_logs__ && mkdir __docker_compose_logs__ && nohup docker compose up --remove-orphans --build >> __docker_compose_logs__/out 2>> __docker_compose_logs__/error &

# Down Docker Compose
docker compose down

# Clean Docker cache
docker builder prune -a --filter until=24h


# ---------------------------Linux commands

# Uvicorn run command
uvicorn main:app --host 0.0.0.0 --port 1020 --reload
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup uvicorn main:app --host 0.0.0.0 --port 8000 --reload >> __public_logs__/out 2>> __public_logs__/error & echo $! > task_id.txt

# Streamlit command
streamlit run app.py --server.port 8115
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup streamlit run app.py --server.port 8115 >> __public_logs__/out 2>> __public_logs__/error & echo $! > task_id.txt

# Nohup command examples
# Without task_id.txt
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup python -u app.py >> __public_logs__/out 2>> __public_logs__/error &
# With task_id.txt
rm -rf __public_logs__ log_dir && mkdir __public_logs__ && nohup python -u app.py >> __public_logs__/out 2>> __public_logs__/error & echo $! > task_id.txt

# Kill nohup process
kill -9 `cat task_id.txt`

# Get used port details
sudo netstat -ltup | grep 8111
# or
sudo netstat -nlp | grep :8111

# Kill port
sudo kill -9 $(lsof -i:8000 -t)

# Get host IP address
curl icanhazip.com

# Nginx update conf
sudo echo "server {
    listen 8014;
    server_name $(curl icanhazip.com);
    location / {
        proxy_pass http://127.0.0.1:8015;
    }
}" > /etc/nginx/sites-enabled/fastapi_nginx

# Celery run command
celery -A file_name worker --loglevel=info
celery -A file_name worker --loglevel=info --logfile=log_dir/celery.log
nohup celery -A file_name worker --loglevel=info >> log_dir/out 2>> log_dir/error & echo $! > task_id.txt

# Flower run command
celery -A file_name flower --port=5001
