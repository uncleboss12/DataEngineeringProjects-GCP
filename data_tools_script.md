```bash
gcloudbeta services identity create --service=dataprep.googleapis.com
```

```
replacepatterns col: * with: '' on: `{start}"|"{end}` global: true
```
```
rename type: manual mapping: [column24,'Candidate_Name'], [column2,'Candidate_ID'],[column8,'Party_Affiliation'], [sum_column16,'Total_Contribution_Sum'], [average_column16,'Average_Contribution_Sum'], [countif,'Number_of_Contributions']
```
`set col: Average_Contribution_Sum value: round(Average_Contribution_Sum)`

///////////////////////////////////////////////////////////////////////
### Dataflow: Qwik Start - Templates
```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
bq mk taxirides
```
```bash
bq mk \
--time_partitioning_field timestamp \
--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
passenger_count:integer -t taxirides.realtime
```
```bash
export BUCKET_NAME=qwiklabs-gcp-00-bb52b858e4f7
```
```bash
gsutil mb gs://$BUCKET_NAME/
```
```bash
ride_id:string,point_idx:integer,latitude:float,longitude:float,timestamp:timestamp,
meter_reading:float,meter_increment:float,ride_status:string,passenger_count:integer
```
```bash
gcloud dataflow jobs run iotflow \
    --gcs-location gs://dataflow-templates-europe-west1/latest/PubSub_to_BigQuery \
    --region europe-west1 \
    --worker-machine-type e2-medium \
    --staging-location gs://qwiklabs-gcp-00-bb52b858e4f7/temp \
    --parameters inputTopic=projects/pubsub-public-data/topics/taxirides-realtime,outputTableSpec=qwiklabs-gcp-00-bb52b858e4f7:taxirides.realtime
```
```bash
SELECT * FROM `qwiklabs-gcp-00-bb52b858e4f7.taxirides.realtime` LIMIT 1000
```
///////////////////////////////////////////////////////////////////////////////////////
### Dataflow: Qwik Start - Python
```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
gcloud config set compute/region us-east4
```
```bash
export BUCKET_NAME=qwiklabs-gcp-00-bb52b858e4f7
```
```bash
gsutil mb gs://$BUCKET_NAME/
```
```bash
docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID python:3.9 /bin/bash
```
```bash
pip install 'apache-beam[gcp]'==2.42.0
```
```bash
python -m apache_beam.examples.wordcount --output OUTPUT_FILE
```
```bash
ls
```
```bash
BUCKET=gs://qwiklabs-gcp-00-5285b3d9a2b2
```
```bash
python -m apache_beam.examples.wordcount --project $DEVSHELL_PROJECT_ID \
  --runner DataflowRunner \
  --staging_location $BUCKET/staging \
  --temp_location $BUCKET/temp \
  --output $BUCKET/results/output \
  --region us-east4
```

//////////////////////////////////////////////////////////////////////////
### Dataproc: Qwik start - Console/ Command Line

```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
gcloud config set dataproc/region us-east4
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
gcloud dataproc clusters create example-cluster --worker-boot-disk-size 500 --worker-machine-type=e2-standard-4 --master-machine-type=e2-standard-4
```
```bash
gcloud dataproc jobs submit spark --cluster example-cluster \
  --class org.apache.spark.examples.SparkPi \
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- 1000
```
```bash
gcloud dataproc clusters update example-cluster --num-workers 4
```
```bash
gcloud dataproc clusters update example-cluster --num-workers 2
```




### Cloud Natural Language API: Qwik Start
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
```
```bash
gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'." > result.json
```
```bash
cat result.json
```

### Speech-to-Text API: Qwik Start


```bash
export API_KEY=<YOUR_API_KEY>
```
```bash
touch request.json
```
```bash
nano request.json
```
```nano
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
```
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
```

### Video Intelligence: Qwik Start
```bash
gcloud auth list
```
```bash
gcloud config list project
```
```bash
gcloud iam service-accounts create quickstart
```
```bash
gcloud iam service-accounts keys create key.json --iam-account quickstart@qwiklabs-gcp-00-6bc6f59d669b.iam.gserviceaccount.com
```
```bash
gcloud auth activate-service-account --key-file key.json
```
```bash
cat > request.json <<EOF
{
   "inputUri":"gs://spls/gsp154/video/train.mp4",
   "features": [
       "LABEL_DETECTION"
   ]
}
EOF
```
```bash
curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @request.json
```
```bash
curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/projects/PROJECTS/locations/LOCATIONS/operations/OPERATION_NAME'
```

```bash
gcloud dataflow jobs run JOB_NAME \
    --gcs-location gs://dataflow-templates/latest/Word_Count \
    --region REGION_NAME \
    --parameters \
    inputFile=gs://dataflow-samples/shakespeare/kinglear.txt,\
    output=gs://BUCKET_NAME/output/my_output,\
    javascriptUdfName=your_udf_name,\
    javascriptUdfPath=gs://path/to/your/udf.js,\
    machineType=n1-standard-4
```


