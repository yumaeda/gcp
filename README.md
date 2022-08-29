# GCP
## Google Cloud SDK
### Install
```zsh
brew install --cask google-cloud-sdk
```
### Setup
```zsh
echo "source '/usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/path.zsh.inc'" >> ~/.zshrc
echo "source '/usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/completion.zsh.inc'" >> ~/.zshrc

source ~/.zshrc
```
### Login
```zsh
gcloud auth login
```
- Credential file is stored to `~/.config/gcloud/application_default_credentials.json`.

### Config
```zsh
gcloud config set project ${PROJECT_ID}
gcloud config set compute/region ${REGION}
gcloud config set compute/zone ${ZONE}
```
### Create `Artifact Registry`
```zsh
gcloud artifacts repositories create ${REPOSITORY} \
    --repository-format=docker \
    --location=${REGION} \
    --description="Docker container image repository"
```
## Configure Docker
```zsh
gcloud auth configure-docker ${REGION}-docker.pkg.dev
```


### Create Deployment (w/ replicas=2)
```zsh
kubectl create deployment ${DEPLOYMENT} --image=${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY}/${IMG_NAME}:${IMG_VERSION}

kubectl scale deployment ${DEPLOYMENT} --replicas=2

kubectl autoscale deployment ${DEPLOYMENT} --cpu-percent=80 --min=1 --max=2
```

### Expose Deployment via LoadBalancer
```zsh
kubectl expose deployment ${DEPLOYMENT} --name=${SERVICE} --type=LoadBalancer --port 80 --target-port 8080
```

&nbsp;

## Misc
### List Credentialed accounts
```zsh
gcloud auth list
```

### Change active account
```zsh
gcloud config set account {Account_Email}
```

### Delete `Artifact Registry`
```zsh
gcloud artifacts repositories delete ${REPOSITORY}
```
### Delete GKE cluster
```zsh
gcloud container clusters delete ${CLUSTER_NAME}
```
### Enable Cloud Spanner service
```zsh
gcloud services enable spanner.googleapis.com
```

&nbsp;

### Cloud Domains
- [Cloud Domains](https://console.cloud.google.com/net-services/domains/registrations/list)

&nbsp;

## References
- https://zenn.dev/chameleon/articles/eb34c3c76f36bd

## TODOs
- https://www.padok.fr/en/blog/gcs-gke-migration
