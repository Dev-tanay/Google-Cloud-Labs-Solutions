# Entity and Sentiment Analysis with the Natural Language API

## Navigation Menu > APIs & Services > Credentials > Create Credentials > API key > Close

## Run in cloudshell 
```cmd
gcloud beta compute ssh  linux-instance
```

### Press y Enter > Enter > Enter > Press n Enter
```cmd
gcloud services enable apikeys.googleapis.com
gcloud alpha services api-keys create --display-name="Tanay" 
KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=Tanay")
API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")
echo $API_KEY
touch request.json
tee request.json <<EOF
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"Joanne Rowling, who writes under the pen names J. K. Rowling and Robert Galbraith, is a British novelist and screenwriter who wrote the Harry Potter fantasy series."
  },
  "encodingType":"UTF8"
}
EOF
cat request.json
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" -s -X POST -H "Content-Type: application/json" --data-binary @request.json > result.json
cat result.json
```
