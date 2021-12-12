MLFlow_churnmodel
==============================

This is a implementation of MLFlow and DVC on test basis, all iplementation derived from thro cookiecutter tempolaate 

Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   ├── predict_model.py
    │   │   └── train_model.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io


--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>


<p>
my command and steps:

create an empty project with python 3.7 and install following:

1. 
pip install cookiecutter
cookiecutter https://github.com/drivendata/cookiecutter-data-science
cd churn_model

--> will get all the required template for the Data Science project code

2. download the raw data from Kaggle git commit and push code to remote origin in the mail branch:

git init
git add . && git commit -m "first commit and added raw data"
git branch -M main
git remote set-url origin https://github.com/genbid007-ml/churn_model.git
git push -u origin main

3. Install DVC for data versioning control with access to google drive:

pip install dvc
pip install dvc[gdrive]


! dvc add Training_Batch_Files/*.csv Prediction_Batch_files/*.csv
!!! Warning Above command will not work for windows users so they can create and run the following file in the root of their project

# NOTE: For windows user-
# This file must be created in the root of the project as dvc_gdrive.py
# where Training and Prediction batch file as are present

import os
from glob import glob


data_dirs = ["/data/external", "/data/processed"]

for data_dir in data_dirs:
    files = glob(data_dir + r"/*.csv")
    for filePath in files:
        # print(f"dvc add {filePath}")
        os.system(f"dvc add {filePath}")

print("\n #### all files added to dvc ####") 


command: Add remote storage
dvc remote add -d storage gdrive://1kGIyJJT3yKK6VehWsQA_4AXDplwzjzZg

git add .dvc/config && git commit -m "Configure remote storage"



4. greate new dev branch and switch for developement

git checkout -b dev

5. 
Push the data to the remote storage-
dvc push
•	This step will ask you to authenticate yourself by clicking on the link which will appear in the terminal.
•	Once you allow dvc to read and write on gdrive it'll give an access token which you'll paste in the terminal.
•	Now the copy of your data will be pushed to the gdrive
•	Above step will create a gdrive credential file (Now check next step).

it will ask to get your access code from gmail credential:

mine is: 4/1AX4XfWgFYu1KCim5K-hkYCHp3p9Wmh78bVrjwTD8aWI6bUkoChxyaPY9tXM

6. Add Gdrive credential secrets in github repo secrets.
•	Find this credentials in the given path -
.dvc >> temp >> gdrive-user-credentials.json
•	Now to add the secrets in your github repo -
o	Go to settings
o	secrets
o	Click on add secrets
o	Give name of secretes
o	Paste the json file content from gdrive-user-credentials.json


7. Create the source code inside the src folder

All python scripts related to the projects are located in the src folder. There are 4 folders namely data, features, visualization, models, and prediction within the src folder. But in this project, I will be using only data, models, and the prediction folders. Also params.yaml file need to be created inside the main churn_model folder.


Data: Data loading related python scripts 
(load_data.py, split_data.py)

Models: Model-related python scripts 
(train_model.py, production_model_selection.py, model_monitor.py)

•	params.yaml
This will store all the configurations related to this project.

•	load_data.py
External train.csv file will be loaded into the raw folder in this script. Only 6 numerical features were used in this model for simplicity. The new CSV which is in the raw folder contains the six numerical features and the target column churn. The main focus of this project is to give more details about the MLOps tools. Therefore very little effort was done for the modeling part.

•	split_data.py
The objective of this python script is to split the train.csv in a raw folder and create new churn_train and churn_test inside the processed folder.

•	train_model.py
Model training will be done using this script. Model-related information is available in params.yaml file. We can experiment with several ML models by changing and adding the parameters in params.yaml file. The mlflow will be used to track the model performances. We can easily check the model performance using the mlflow dashboard.

•	production_model_selection.py
This script will support you to select the best-performing model from the model registry and save the best model in the model directory. The best model was selected using an accuracy score in this model.


8. Pipeline Creation
After creating the above files in src, Now it's time to write the model 
pipeline to execute the model. DVC will be used to create the model pipeline. 
For that, first, create the dvc.yaml file inside the churn_model folder.

9. Pipeline Execution
The next step is to execute the model pipeline. Now we are going to execute the dvc.yaml file. It contains four stages. Each stage contains at least three steps
 cmd: command used to execute the script
 deps: specify the dependencies to execute the step.
 outs: output from the step(Model files or datasets).
 params: parameters used in the script.
10. install MLFLow
 conda install -c conda-forge mlflow
 
start MLFLOW:
mlflow server --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./artifacts --host 127.0.0.1 -p 9002

11. dvc repro
add your remote MLFLOW server uri at http//127.0.0.1:9002 in params.yaml

To track the changes with git, run:

    git add dvc.lock 

To enable auto staging, run:

        dvc config core.autostage true
Use `dvc push` to send your updates to remote storage.


12. to open mlflow ui go to 127.0.0.1:9002

13. Web app in flask
This web app will be used to consume the created model. A user can enter the feature values into the form and after submitting, the model will predict the outcome(churn or not).
Create a new folder named webapp and put the required HTML, CSS, and JavaScript codes inside the folder
Now it’s time to create the python code related to the web app. Create app.py file in chrun_folder. The objective of this script is to send the response to the frontend after predicting the target using the request.


14. Unite test cases
 Pytest will be used to do the simple testing. For that, create a separate folder named tests inside the main directory. Then create the test_config.py and __Init__.py files inside the newly created folder. Here a simple test will be performed to check whether the entered values are numerical or not. It will raise an exception if someone enters a string value instead of a numerical value. It is important to remember that the function name of all test cases must start with the test. After creating the unit tests we can test them by executing the below command. We can also do this as a frontend validation. But, here I did it in the backend just to show the unit tests capabilities of python. Here it checks whether the required error message is passed when entering incorrect values into the form. (Ex. adding one or more non-numerical values)

run:
pytest -v 

15 Heroku Deployement
Go to https://dashboard.heroku.com/apps
* Click New and create a new app
* Give a name for the app and create it (I named it churnmodelprod)
* In the deployment method section, click Github.
* In the connect to Github section, enter the repo name and search. It will find the repo name for you. Then click connect.
* In the automatic deployed section, tick Wait for CI to pass before deploying and click enable the automatic deploy button.
* Then go to account setting → application → Authorizations → create authorization.
* Then enter the description in the box and click create.
* Copy and save the authorization token for future use (We need this in the next step to create secrets).


15. Create CI-CD pipeline using GitHub actions
CI-CD pipeline will be created using the GitHub actions. 
Create a new file inside the .github/workflows folder and named it as the ci-cd.yaml. 
Please note that the file extension of this must be yaml.
We can easily reflect the changes in the model or code through the frontend after implementing the CI-CD pipeline. Because we just need to push the code after doing the modifications and it will reflect the changes automatically. That is the advantage of using the CI-CD pipeline for ML model development. Because in ML context, there is a model retraining part that is not included in the normal software development life cycle(We will be discussing the retraining part in the 13th section). We can easily reflect the changes to the model with this approach.

16 . open github repo add following:

Now we need to create two secrets inside GitHub as HEROKU_API_TOKEN and HEROKU_API_NAME to do the deployment.
* Select the repo and click the settings.
* Then click secrets in the left panel.
* Click new repository secrets. Need to create two secrets.
1. name: HEROKU_API_NAME |value: churngenbid
2. name: HEROKU_API_TOKEN |value: Authorization token saved in the last step

</p>