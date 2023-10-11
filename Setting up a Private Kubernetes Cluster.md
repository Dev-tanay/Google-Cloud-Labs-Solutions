# Setting up a Private Kubernetes Cluster

## Run in cloudshell (Zone from Task 1 step 3)
```cmd
export ZONE=
```

```cmd
gcloud config set compute/zone $ZONE
export REGION=${ZONE%-*}
gcloud beta container clusters create private-cluster \
    --enable-private-nodes \
    --master-ipv4-cidr 172.16.0.16/28 \
    --enable-ip-alias \
    --create-subnetwork ""
gcloud compute instances create source-instance --zone=$ZONE --scopes 'https://www.googleapis.com/auth/cloud-platform'
NAT_IAP_TANAY=$(gcloud compute instances describe source-instance --zone=$ZONE | grep natIP | awk '{print $2}')
gcloud container clusters update private-cluster \
    --enable-master-authorized-networks \
    --master-authorized-networks $NAT_IAP_TANAY/32
gcloud container clusters delete private-cluster --zone=$ZONE --quiet
gcloud compute networks subnets create my-subnet \
    --network default \
    --range 10.0.4.0/22 \
    --enable-private-ip-google-access \
    --region=$REGION \
    --secondary-range my-svc-range=10.0.32.0/20,my-pod-range=10.4.0.0/14
gcloud beta container clusters create private-cluster2 \
    --enable-private-nodes \
    --enable-ip-alias \
    --master-ipv4-cidr 172.16.0.32/28 \
    --subnetwork my-subnet \
    --services-secondary-range-name my-svc-range \
    --cluster-secondary-range-name my-pod-range \
    --zone=$ZONE
NAT_IAP_Cloud=$(gcloud compute instances describe source-instance --zone=$ZONE | grep natIP | awk '{print $2}')
gcloud container clusters update private-cluster2 \
    --enable-master-authorized-networks \
    --zone=$ZONE \
    --master-authorized-networks $NAT_IAP_Cloud/32
```
