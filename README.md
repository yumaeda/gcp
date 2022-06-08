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
### Set environment variables
```zsh
export PROJECT_ID=hello-world-xxxxxx
export IMG_NAME=hello-world
export IMG_VERSION=1.0.1
export REGION=us-central1
export ZONE=us-central1-a
export CLUSTER_NAME=hello-world
export REPOSITORY=hello-world
export DEPLOYMENT=hello-world
export SERVICE=hello-world-service
```
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
### Create GKE cluster
- From GKE 1.12.0, [`f1-micro` machine type is no longer supported](https://stackoverflow.com/questions/61357217/gcloud-kubernetes-in-f1-micro-results-in-node-pools-of-f1-micro-machines-are-no).

```zsh
gcloud services enable container.googleapis.com

gcloud container clusters create \
  --preemptible \
  --project=${PROJECT_ID} \
  --machine-type g1-small \
  --num-nodes 3 \
  --disk-size 10 \
  --zone ${ZONE} \
  --cluster-version latest \
  ${CLUSTER_NAME}
```
### Check connection to the GKE cluster
```zsh
gcloud container clusters get-credentials ${CLUSTER_NAME}
```
### Get a list of GKE cluster
```zsh
gcloud container clusters list
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
### Delete `Artifact Registry`
```zsh
gcloud artifacts repositories delete ${REPOSITORY}
```
### Delete GKE cluster
```zsh
gcloud container clusters delete ${CLUSTER_NAME}
```
## References
- https://zenn.dev/chameleon/articles/eb34c3c76f36bd
