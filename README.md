# MLflow On-Premise Deployment using Docker Compose
Easily deploy an MLflow tracking server with 1 command.

MinIO S3 is used as the artifact store and MySQL server is used as the backend store.

## How to run
1. Clone(download) this repository
    ```
    git clone https://github.com/sachua/mlflow-docker-compose.git
    ```
2. `cd` into the `mlflow-docker-compose` directory
3. Build and run the containers with `docker-compose`
    ```
    docker-compose up -d --build
    ```
4. Access MLflow UI with http://localhost
5. Access MinIO UI with http://localhost:9000

## Containerization
The MLflow tracking server is composed of 4 docker containers:

* MLflow server
* MinIO object storage server
* MySQL database server
* NGINX reverse proxy
