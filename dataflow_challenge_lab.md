# Week 6 Lab


## Pub/Sub: Qwik Start - Console

```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
gcloud pubsub subscriptions pull --auto-ack MySub
```

## Streaming Analytics into BigQuery: Challenge Lab
```bash
export BUCKET_NAME=
```

```bash
export BIGQUERY_DATASET_NAME=
```
```bash
export TABLE_NAME=
```
```bash
export TOPIC_NAME=
```
```bash
gsutil mb gs://$BUCKET_NAME/
```
```bash
bq mk $BIGQUERY_DATASET_NAME
```
```bash
bq mk \
--schema data:string -t $BIGQUERY_DATASET_NAME.$TABLE_NAME
```
```bash
gcloud pubsub topics create $TOPIC_NAME
```
```bash
gcloud pubsub subscriptions create $TOPIC_NAME-sub --topic $TOPIC_NAME
```
```bash
gcloud dataflow jobs run dfjob-69172 \
    --gcs-location gs://dataflow-templates-"europe-west1"/latest/PubSub_to_BigQuery \
    --region "europe-west1" \
    --worker-machine-type e2-medium \
    --staging-location gs://"qwiklabs-gcp-01-e8330c241dbb"/temp \
    --parameters inputTopic="sensors-temp-13610",outputTableSpec="Table Name":sensors_978.temperature_745
```

```bash
gcloud dataflow jobs run dfjob-69172 \
    --gcs-location gs://dataflow-templates-europe-west1/latest/PubSub_to_BigQuery \
    --region europe-west1 \
    --worker-machine-type e2-medium \
    --staging-location gs://qwiklabs-gcp-01-e8330c241dbb/temp \
    --parameters inputTopic=projects/PROJECT_ID/topics/sensors-temp-13610,outputTableSpec=Table_Name:sensors_978.temperature_745
```

```bash
gcloud dataflow jobs run $JOB_NAME-1 --gcs-location gs://dataflow-templates-$REGION/latest/PubSub_to_BigQuery --region $REGION --staging-location gs://$DEVSHELL_PROJECT_ID/temp --parameters inputTopic=projects/$DEVSHELL_PROJECT_ID/topics/$TOPIC_NAME,outputTableSpec=$DEVSHELL_PROJECT_ID:$BIGQUERY_DATASET_NAME.$TABLE_NAME
```

```bash
export REGION=europe-west1
export DATASET=sensors_119
export TABLE=temperature_134
export TOPIC=sensors-temp-21703
export JOB=dfjob-40578
```
```bash
curl -LO raw.githubusercontent.com/QUICK-GCP-LAB/2-Minutes-Labs-Solutions/main/Streaming%20Analytics%20into%20BigQuery%20Challenge%20Lab/arc106.sh
```
```bash
sudo chmod +x arc106.sh
```
```bash
./arc106.sh
```
