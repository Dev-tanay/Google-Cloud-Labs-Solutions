# App Engine: Qwik Start - Go

### Select your Region from task 3
```cmd
export REGION=
```

## Run in cloudshell
```cmd
gcloud config set compute/region $REGION
gcloud services enable appengine.googleapis.com
git clone https://github.com/GoogleCloudPlatform/golang-samples.git
cd golang-samples/appengine/go11x/helloworld
sudo apt-get install google-cloud-sdk-app-engine-go
gcloud app deploy
gcloud app browse
```

### Select the Region from task 3 and press y when asked
