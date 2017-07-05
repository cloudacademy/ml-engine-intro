# Introduction to Google Cloud Machine Learning Engine
This file contains text you can copy and paste for the examples in Cloud Academy's _Introduction to Google Cloud Machine Learning Engine_ course.  

### TensorFlow
TensorFlow website: https://www.tensorflow.org  
High-level API (tf.contrib.learn) tutorial: https://www.tensorflow.org/get_started/tflearn  
TensorFlow installation: https://www.tensorflow.org/install  

Add to iris.py:  
```
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
tf.logging.set_verbosity(tf.logging.ERROR)
import shutil
shutil.rmtree('/tmp/iris_model', ignore_errors=True)
```

```
python iris.py  
```

### Training a Model with ML Engine
Google Cloud SDK installation: https://cloud.google.com/sdk  

```
mkdir trainer  
mv Downloads/iris.py trainer  
touch train/__init__.py  
gcloud ml-engine local train --module-name trainer.iris --package-path trainer  
```

```
BUCKET=gs://[ProjectID]-ml  # Replace [ProjectID] with your Google Cloud Project ID  
REGION=[Region]  # Replace [Region] with a Google Cloud Platform region, such as us-central1  
```
```
gcloud ml-engine jobs submit training job1
    --module-name trainer.iris 
    --package-path trainer 
    --staging-bucket $BUCKET 
    --region $REGION  
```

### Feature Engineering
TensorFlow Linear Model Tutorial: https://www.tensorflow.org/tutorials/wide  

Add to wide_n_deep_tutorial.py:  
```
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
tf.logging.set_verbosity(tf.logging.ERROR)
```

```
sudo pip install pandas  
```

```
python wide_n_deep_tutorial.py --model_type=wide  
```

### A Wide and Deep Model
```
python wide_n_deep_tutorial.py --model_type=deep  
```

### Distributed Training on ML Engine
Hyperparameter Tuning: https://cloud.google.com/ml-engine/docs/concepts/hyperparameter-tuning-overview  

```
gsutil cp -r gs://cloudml-public/census/data $BUCKET  
TRAIN_DATA=$BUCKET/data/adult.data.csv  
EVAL_DATA=$BUCKET/data/adult.test.csv  
JOB=census1  
```
```
gcloud ml-engine jobs submit training $JOB \  
    --job-dir $BUCKET/$JOB \  
    --runtime-version 1.2 \  
    --module-name trainer.task \  
    --package-path trainer/ \  
    --region $REGION \  
    --scale-tier STANDARD_1 \  
    -- \  
    --train-files $TRAIN_DATA \  
    --eval-files $EVAL_DATA \  
    --train-steps 1000 \  
```

### Deploying a Model on ML Engine
```
gcloud ml-engine models create census --regions=$REGION  
gsutil ls -r $BUCKET/census1/export  
```
```
# Note: Replace [Path-to-model] below with your Cloud Storage path
gcloud ml-engine versions create v1 \  
    --model census \  
    --origin gs://[Path-to-model] \    
    --runtime-version 1.2  
```
```
gcloud ml-engine predict \  
    --model census \  
    --version v1 \  
    --json-instances \  
    ../test.json  
```

### Conclusion
Cloud Machine Learning Engine documentation: https://cloud.google.com/ml-engine/docs  
Cloud Academy community forums: http://cloudacademy.com/community  
