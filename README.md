# MLflow On-Premise Deployment using Docker Compose
Easily deploy an MLflow tracking server with 1 command.

MinIO S3 is used as the artifact store and MySQL server is used as the backend store.

## How to run

1. Clone (download) this repository

    ```bash
    git clone https://github.com/sachua/mlflow-docker-compose.git
    ```

2. `cd` into the `mlflow-docker-compose` directory

3. Build and run the containers with `docker-compose`

    ```bash
    docker compose up -d --build
    ```

4. Access MLflow UI with http://localhost:5000

5. Access MinIO UI with http://localhost:9000

## Containerization

The MLflow tracking server is composed of 3 docker containers:

* MLflow server
* MinIO object storage server
* MySQL database server

## Example

1. Install [conda](https://conda.io/projects/conda/en/latest/user-guide/install/index.html)

2. Install MLflow with extra dependencies, including scikit-learn

    ```bash
    pip install mlflow boto3
    ```

3. Set environmental variables

    ```bash
    export MLFLOW_TRACKING_URI=http://localhost:5000
    export MLFLOW_S3_ENDPOINT_URL=http://localhost:9000
    ```
4. Set MinIO credentials

    ```bash
    cat <<EOF > ~/.aws/credentials
    [default]
    aws_access_key_id=minio
    aws_secret_access_key=minio123
    EOF
    ```

5. Train a sample MLflow model

    ```bash
    mlflow run https://github.com/sachua/mlflow-example.git -P alpha=0.42
    ```

 6. Serve the model (replace ${MODEL_ID} with your model's ID)
    ```bash
    export MODEL_ID=0ced24069348417fbbcb2cd41a7d2f07 # Replace this with your model's ID
    mlflow models serve -m runs:/${MODEL_ID}/model -p 1234 --env-manager conda
    ```

 7. You can check the input with this command
    ```bash
    curl -X POST -H "Content-Type:application/json" --data '{"dataframe_split":{"columns":["fixed acidity", "volatile acidity", "citric acid", "residual sugar", "chlorides", "free sulfur dioxide", "total sulfur dioxide", "density", "pH", "sulphates", "alcohol"],"data":[[6.2, 0.66, 0.48, 1.2, 0.029, 29, 75, 0.98, 3.33, 0.39, 12.8]]}}' http://127.0.0.1:1234/invocations
    ```
