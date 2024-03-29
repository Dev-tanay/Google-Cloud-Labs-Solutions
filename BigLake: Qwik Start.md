# BigLake: Qwik Start

## Run this in cloudshell
```cmd
bq mk --connection --location=US --project_id=$DEVSHELL_PROJECT_ID \
    --connection_type=CLOUD_RESOURCE my-connection
SERVICE_ACCOUNT_ID=$(bq show --format=json --connection $DEVSHELL_PROJECT_ID.US.my-connection | jq -r '.cloudResource.serviceAccountId')
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member="serviceAccount:$SERVICE_ACCOUNT_ID" --role="roles/storage.objectViewer"
bq mk demo_dataset
bq mkdef --source_format=CSV --autodetect=true \
  gs://$DEVSHELL_PROJECT_ID/customer.csv > mytable_def
bq mk --table --external_table_definition=mytable_def \
  demo_dataset.biglake_table
bq mkdef --source_format=CSV --autodetect=true \
  gs://$DEVSHELL_PROJECT_ID/invoice.csv > mytable_deff
bq mk --table --external_table_definition=mytable_deff \
  demo_dataset.external_table
bq mkdef \
--autodetect \
--connection_id=$DEVSHELL_PROJECT_ID.US.my-connection \
--source_format=CSV \
"gs://$DEVSHELL_PROJECT_ID/invoice.csv" > /tmp/tabledef.json
bq show --schema --format=prettyjson  demo_dataset.external_table > /tmp/schema
bq update --external_table_definition=/tmp/tabledef.json --schema=/tmp/schema demo_dataset.external_table
```
