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
