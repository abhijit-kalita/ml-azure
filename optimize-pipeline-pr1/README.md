# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
The data is related to a bank marketing campaign. 
Based on data collected on the potential subscribers to the campaign
the objective of the campaign was to use a classfication algorithm to train
the data to predict the likelyhood of a customer subscribing
to the sampaign ( Y or N prediction).


## Solution: 
The best performing model was 


## Scikit-learn Pipeline
## As SK learn estimator is dericated as ScriptRunConfig estimator is used instead
The approach to solve the problem was based on two approaches
and then compare the results:
## First approach:
Classical approach using a manual configuration in which 
a hyperparameter sampling strategy was specified .In this case
Random sampling. 
Then an early stopping policy was defined to mark the boundaries
of run. In this case BAdnit Policy was used as it terminates 
any run which are not within slack factor or slack amount with
respect to best performing run
A hyberprive config object defined with these parameters 
And finally the experiment submitted in a provisioned 
standard cluster with a maximum of 12 runs. Less number
of runs chosen in order to finish faster as high accuracy wasnt
of main importance for the exercise
## Second approach:
AutoML approach : An automl config job was created with classfication
task type , primary metric of accuracy and the targetted column
of Y/N. The explanation feature for the best trained model was
also turned on. 
Based on these two approaches two best models were obtained 
and we could do a comparison between the two to determine
which approach gave a better model in this case.

Data: The data is as specifid earlier is related to a bank marketing
campaign. The data contains features like jb, age, marital status,
education, whether previously targetted or not , means used
to contact them , day of the week and subscribed to the campaign.
The default data view and data profile was used to check
for the data characteristics

Hyperparamaters: For the manual config , the number of iterations
and inverse of regularization strength. A range was was given
and these were optimized using hyperdrive. Random sampler was used
as the sampler method in order to have random samples 
from the sample space and in the defined limits because accuracyis not of 
high importance for the exercise and this would be faster
which also giving good results.

Classification algorithms: 
Classical manual approach: MaxabsScaler Light GBM, MaxAbsScaler
XGBosstClassifier, SparseNormalizer RandomForest etc
AutoMl: Some of the algorithms used for the automl were VotingEnsemble,
StackEnsemble, MaxAbsScaler, LightGBM, XgBoostClassifier etc
VotingEnsemble gave the best results in this.

Pipeline Architecture: 
For the data ingestion , the data was ingested to azml workspace 
using the given data url. 

A train step is created with a train.py script which makes the 
training step modularised and easily reusable.
The data cleaning and processing happens in this step
with dictionary and hot encoding
Inverse of regularization and max number of iterations are
specified hyperparameterization. A not so high value of
number of iterations is  used as accuracy is not of paramount importance
and resources scarce.Smaller values of Inverse of regularization cause
stronger regularization.

A RandomSampler specified with an early stopping policy
A ScriptRunConfig estimator object is created with the training 
file, compute target and environment ocjects as principal inputs.
Finally a Hyperdrive pipeline object is created with above 
ScriptRunConfig object, metric to optimise/maximize, policy, number of runs 

For the automl run, a pipeline config is created of classification
task type, primary metric as accuracy , the input training data 
and targetted column for prediction . Also explanation mode is turned on.
And at the end of the run we a list of models with the best models also having an explanation attached.


## Pipeline comparison
After the completion of the ml runs with the two processes
it is seen that the auto ml run performed a little better.
Though the difference was marginal.
Hyberprive accuracy:
Automl accuracy:

## Future work
For this project the objective was to see the the overall
pipeline optimization steps using different appriaches
with use of minimal recources and in the process obtain what
best results are possible.
If there was more time then better results could perhaps be obtained.
Some of the things whicch can be done are
Better prepare the data . Make it more balanced for example: Will lead to better input data which will result in better trained model
Use other sampling methods such as grid sampling which will lead to more hyperparameters being used to get possibly better results
Increase number of iterations for optimization of target feature which wll allow more steps for maximizing resukts though adfter a certain number of steps this can remain constant
Use different ML algrorithms and estimators which provide other basic approaches and probably better results


