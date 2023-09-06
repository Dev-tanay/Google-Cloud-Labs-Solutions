# Extract, Analyze, and Translate Text from Images with the Cloud ML APIs

## Run in cloudshell
```cmd
gcloud alpha services api-keys create --display-name="Tanay" 
KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=Tanay")
API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
gsutil mb -p $PROJECT_ID -c regional -l us-central1 gs://$PROJECT_ID
curl -O https://github.com/siddharth7000/practice/blob/main/sign.jpg
gsutil cp sign.jpg gs://$PROJECT_ID/sign.jpg
gsutil acl ch -u AllUsers:R gs://$PROJECT_ID/sign.jpg
touch ocr-request.json
tee ocr-request.json <<EOF
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://$PROJECT_ID/sign.jpg"
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
curl -s -X POST -H "Content-Type: application/json" --data-binary @ocr-request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
curl -s -X POST -H "Content-Type: application/json" --data-binary @ocr-request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o ocr-response.json
touch translation-request.json
tee translation-request.json <<EOF
{
  "q": "My Name is CLOUDHUSTLER",	
  "target": "en"
}
EOF
STR=$(jq .responses[0].textAnnotations[0].description ocr-response.json) && STR="${STR//\"}" && sed -i "s|your_text_here|$STR|g" translation-request.json
curl -s -X POST -H "Content-Type: application/json" --data-binary @translation-request.json https://translation.googleapis.com/language/translate/v2?key=${API_KEY} -o translation-response.json
cat translation-response.json
touch nl-request.json.json
tee nl-request.json <<EOF
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"your_text_here"
  },
  "encodingType":"UTF8"
}
EOF
STR=$(jq .data.translations[0].translatedText  translation-response.json) && STR="${STR//\"}" && sed -i "s|your_text_here|$STR|g" nl-request.json
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @nl-request.json
```
