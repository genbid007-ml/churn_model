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

</p>