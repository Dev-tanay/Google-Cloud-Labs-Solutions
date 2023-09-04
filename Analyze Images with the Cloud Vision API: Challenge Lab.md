# Analyze Images with the Cloud Vision API: Challenge Lab

## Run this in cloudshell 
```cmd
gcloud alpha services api-keys create --display-name="CloudHustlers" 
KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=CloudHustlers")
export API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
cat > request.json <<EOF
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://$PROJECT_ID-bucket/manif-des-sans-papiers.jpg"
          }
        },
        "features": [
          {
            "type": "TEXT_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
EOF
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o text-response.json
gsutil cp text-response.json gs://$PROJECT_ID-bucket
cat > request.json <<EOF
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://$PROJECT_ID-bucket/manif-des-sans-papiers.jpg"
          }
        },
        "features": [
          {
            "type": "LANDMARK_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
EOF
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o landmark-response.json
gsutil cp landmark-response.json gs://$PROJECT_ID-bucket
```
