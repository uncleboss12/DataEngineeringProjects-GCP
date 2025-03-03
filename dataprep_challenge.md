```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
PROJECT_ID=$(gcloud config get-value project) && \
gcloud config set project $PROJECT_ID
```

```bash
PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)')
```
```bash
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member=serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com \
  --role=roles/storage.admin
```
```bash
bq mk goodtable
```
```bash
bq mk badtable
```
```bash
export BUCKET_NAME=qwiklabs-gcp-00-bb52b858e4f7
```
```bash
gsutil mb gs://$BUCKET_NAME/
```
```bash
gcloud dataflow jobs run simple_job \
    --gcs-location gs://dataflow-templates-us-east1/01/GCS_CSV_to_BigQuery \
    --region us-east1 \
    --parameters \
inputFilePattern=gs://cloud-training/gsp323/lab.csv,\
schemaJSONPath=gs://cloud-training/gsp323/lab.schema,\
outputTable=qwiklabs-gcp-03-9b06bfebeef1:lab_739.customers_533,\
badRecordsOutputTable=qwiklabs-gcp-03-9b06bfebeef1:lab_739.bad_table,\
csvFormat=CSV_FORMAT,\
delimiter=DELIMITER,\
bigQueryLoadingTemporaryDirectory=gs://qwiklabs-gcp-03-9b06bfebeef1-marking/bigquery_temp,\
containsHeaders=CONTAINS_HEADERS,\
csvFileEncoding=CSV_FILE_ENCODING
```


```bash
touch result.json
```
```bash
nano result.json
```
```nano
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-training/gsp323/task3.flac"
  }
}
```


```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
```
```bash
gsutil cp result.json gs://qwiklabs-gcp-04-cb56d9e169b1-marking/task3-gcs-781.result
```

```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
```
```bash
gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"
```
```bash
gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
```
```bash
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"
gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > result.json
```
```bash
gsutil cp result.json gs://qwiklabs-gcp-04-cb56d9e169b1-marking/task4-cnl-106.result
```
