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
### Config
```zsh
gcloud config set project ${PROJECT_ID}
gcloud config set compute/region us-west1
gcloud config set compute/zone us-west1-a
```
### Create `Artifact Registry`
```zsh
gcloud artifacts repositories create hello-world \
    --repository-format=docker \
    --location=us-west1 \
    --description="Docker container image repository"
```
## Configure Docker
```zsh
gcloud auth configure-docker us-west1-docker.pkg.dev
```
### Create GKE cluster
```zsh
gcloud services enable container.googleapis.com

gcloud container clusters create-auto hello-world \
    --region us-west1 \
    --project=${PROJECT_ID}
```
### Check connection to the GKE cluster
```zsh
gcloud container clusters get-credentials hello-world --region us-west1
```
### Get a list of GKE cluster
```zsh
gcloud container clusters list
```
### Create Deployment (w/ replicas=2)
```zsh
kubectl create deployment hello-world --image=us-west1-docker.pkg.dev/${PROJECT_ID}/hello-world/hello-world:v1

kubectl scale deployment hello-world --replicas=2

kubectl autoscale deployment hello-world --cpu-percent=80 --min=1 --max=2
```

### Expose Deployment via LoadBalancer
```zsh
kubectl expose deployment hello-world --name=hello-world-service --type=LoadBalancer --port 80 --target-port 8080
```

&nbsp;

## Misc
### Delete GKE cluster
```zsh
gcloud container clusters delete hello-world
```
## References
- https://zenn.dev/chameleon/articles/eb34c3c76f36bd
