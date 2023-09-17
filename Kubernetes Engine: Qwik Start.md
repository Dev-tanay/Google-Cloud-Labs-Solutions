# Kubernetes Engine: Qwik Start

## Run in cloudshell
```cmd
export ZONE=
```

```cmd
export REGION=${ZONE::-2}
gcloud config set compute/region $REGION
gcloud config set compute/zone $ZONE
gcloud container clusters create lab-cluster --machine-type=e2-medium --zone=$ZONE
gcloud container clusters get-credentials lab-cluster
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
kubectl expose deployment hello-server --type=LoadBalancer --port 8080
```

```cmd
gcloud container clusters delete lab-cluster
```
