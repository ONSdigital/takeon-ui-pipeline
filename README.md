# takeon-ui-pipeline

takeon-ui-pipeline contains the code for the Python Flask UI container pipeline.  This pipeline runs as follows:
1. Triggered by a Git Push to the Dev Branch of the Takeon-UI GitHub repository 

2. Builds a Docker image from this code 

3. Pushes it to Amazon ECR

4. Retrieves Kubernetes Configuration from a s3 Bucket

5. Deploys application to Kubernetes

## Setting up Concourse and a Test pipeline

1. Install Docker.
2. Install Docker Compose if not included in your Docker installation.
3. Deploy Concourse using Docker Compose:
4. Get file from: https://github.com/starkandwayne/concourse-tutorial/blob/master/docker-compose.yml

```bash
docker-compose up -d
```
5. Test: Open http://127.0.0.1:8080/ in your browser
6. Click on your operating system to download the fly CLI.
7. Once downloaded, copy the fly binary into your path ($PATH) or into a directory that is in your PATH.  (I copied mine to C:\\Applications)
```bash
fly --target tutorial login --concourse-url http://127.0.0.1:8080 -u admin -p admin
fly --target tutorial sync
cd ~
open ~/.flyrc
docker-compose down
```

## Usage

```bash
fly --target test-cicd login --concourse-url http://127.0.0.1:8080 -u admin -p admin
fly --target test-cicd sync
fly -t test-cicd set-pipeline -p takeon-ui-pipeline -c test-cicd.yaml --load-vars-from params.yml
fly -t test-cicd unpause-pipeline -p takeon-ui-pipeline
```
To Destroy the pipeline:
```bash
fly -t test-cicd destroy-pipeline -p takeon-ui-pipeline
```
To watch job output in terminal:
```bash
fly -t test-cicd watch -j takeon-ui-pipeline/Docker-Build
```


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)