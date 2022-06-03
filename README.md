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

&nbsp;

## References
- https://zenn.dev/chameleon/articles/eb34c3c76f36bd
