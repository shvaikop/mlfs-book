# mlfs-book
O'Reilly book - Building Machine Learning Systems with a feature store: batch, real-time, and LLMs


## ML System Examples


[Dashboards for Example ML Systems](https://featurestorebook.github.io/mlfs-book/)


# Run Air Quality Tutorial

See [tutorial instructions here](https://docs.google.com/document/d/1YXfM1_rpo1-jM-lYyb1HpbV9EJPN6i1u6h2rhdPduNE/edit?usp=sharing)
    
# Create a conda or virtual environment for your project before you install the requirements
    pip install -r requirements.txt


##  Run pipelines with make commands

    make aq-backfill
    make aq-features
    make aq-train
    make aq-inference
    make aq-clean

or 
    make aq-all



## Feldera


mkdir -p /tmp/c.app.hopsworks.ai
ln -s  /tmp/c.app.hopsworks.ai ~/hopsworks
docker run -p 8080:8080 \
  -v ~/hopsworks:/tmp/c.app.hopsworks.ai \
  --tty --rm -it ghcr.io/feldera/pipeline-manager:latest


## Introduction to ML
I wrote a brief introduction to machine learning [here](./introduction_to_supervised_ml.pdf)



## Homework 1: Air Quality Prediction

### Step 1: Setting up the API keys, secrets in github, and ensuring the pipeline works locally

As the first step we chose the city of Malaga in Spain to focus on. It has 4-5 sensors with
reasonable amount of data to train models on.
The data of the sensors seems to be accurate too.
There is just one sensor [Carril del Capitán, Málaga, Spain](https://aqicn.org/station/@203590/) 
which seems to be ouputting values which seem too good compared to other sensors nearby.
On top of that the sensor value does not seem to be changing.

List of other sensors used:
[Carril del Capitán](https://aqicn.org/station/@203590/)
[Carranque, Malaga](https://aqicn.org/city/spain/andalucia/malaga/carranque/)
[El-Etabal, Malaga](https://aqicn.org/station/spain/andalucia/malaga/el-atabal/)
[Campanillas](https://aqicn.org/station/spain/andalucia/malaga/campanillas/)
[Calle Hernando de Zafra, Centro](https://aqicn.org/station/@128242/)

All the above sensors had adequate amount of data to train models, for most of them historical data dates back to 2019-2020.

Next we setup our Hopsworks accounts and the API Key.
After that we setup the Aqicn account and registered to get the token from there to be able to make API calls.

As the next step we ran through the jupyter notebooks 1-4 locally to make sure everything works. This step went fairly smoothly.

#### Setting up github actions

Both of us were new to github actions but we were excited to get some practice with this technology.
In order for gh actions script to be able to access the API keys and other environment variables we created a new environment in Github.
We added the API keys as secrets to the environment and other environment variables as plain env vars in the environment.

In the yaml file we map the secrets to env vars so they are accessible to the python jupyter notebooks which are run.

At the start github actions were not triggering automatically becasue we didn't know that they only trigger from the default branch.
We also needed to fix the syntax around the cron schedule.

#### Default model performance

The default model performance was fairly poor based on the MSE and the R_squared measures:

MSE: 623.27673

R squared: -3.2823781585240415

![pm25 hindcast](./notebooks/airquality/air_quality_model/no_improvements/pm25_hindcast.png)

![feature_importance](./notebooks/airquality/air_quality_model/no_improvements/feature_importance.png)

#### Tuned hyper params

MSE: 419.45096

R squared: -1.8749323200065513

![pm25 hindcast](./notebooks/airquality/air_quality_model/added_hyper_params/pm25_hindcast.png)

![feature_importance](./notebooks/airquality/air_quality_model/added_hyper_params/feature_importance.png)

The performance improved slightly but not significantly. The model is still performing poorly.
We did not run any hyper parameter tuning algorithm like grid search but we believe that the performance
would not increase significantly because the model is missing information that this is time series data.


### Step 2: Adding lagged features

We decided to add pm25 values from 3 previous days as features of the model when predicting the next day.
This should allow the model to learn the trends from time series data.

The first notebook had to be modified to add the lagged features to the feature store.
The third notebook had to be modified to change the model to use the new features.

The fourth notebook had to be modified to extract the pm25 values for previous days to use as inputs into the model.
When predicting more than 1 day in advance we need to use the previous predictions of the model as inputs when predicting further future days.

We also decided to modify the test/train data split during training to not split on a hardcoded date but to achieve a 80/20 train/test data split.
The ratio can be modified if needed.

#### Vanilla model with lagged features

MSE: 101.65884 
R squared: 0.2958287605921418

Solely from the accuracy metrics we can see the model performs much better with lagged features even when default hyper-params are used.
This is because it now can learn from the time-series nature of the data.

![pm25 hindcast](./notebooks/airquality/air_quality_model/lag_features_plain_xgboost/pm25_hindcast.png)

![feature_importance](./notebooks/airquality/air_quality_model/lag_features_plain_xgboost/feature_importance.png)


#### Tuned hyper-params with lagged features

MSE: 93.04523
R squared: 0.3554935567341714

We can see that tuning hyperparameters slightly improves the performance of the model however, 
it is insignificant compared to the improvement from adding lagged features.

![pm25 hindcast](./notebooks/airquality/air_quality_model/lag_features_hyper_params/pm25_hindcast.png)

![feature_importance](./notebooks/airquality/air_quality_model/lag_features_hyper_params/feature_importance.png)


### Step 3: Parametrizing the notebooks to use them to train models for different sensors

Next, we parametrized the notebooks to be able to provid the sesort url, country, city, ... as arguments.
We used country, city, street in names of feature groups, feature views and in models saved in hopsworks too.







