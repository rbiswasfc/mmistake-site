---
title: How to set up a data science project in GitHub
tags:
    - Data Science
    - Deep Learning
---

# Introduction

I am writing this article for my future reference. It describes a simple getting started template for data science projects, which can later be customized as per specific project requirements. It incorporates a typical structure which helps to organize the files, folders and codes. For most part, I will be following this Github repository: [https: // github.com / equinor / data - science - template](https: // github.com / equinor / data - science - template).


# Set up local Git repository
* Login to your github account and create a new repository. Note the corresponding url e.g. [https: // github.com / rbiswasfc / NLP](https: // github.com / rbiswasfc / NLP).

* Clone the content of[this](https: // github.com / equinor / data - science - template) repository in the project folder e.g. NLP

```$git clone https: // github.com / Statoil / data - science - template NLP```

* Go into the project directory and remove .git folder

```$cd NLP```

```$rm - rf .git```

* Initialize the git repo and make first commit

```$git init```

```$git add - A```

```$git commit - m "First Commit"```

* Add the url of newly created Github repository as remote location for local repo

```$git remote add origin https: // github.com / rbiswasfc / NLP.git```

* Push the content of local repo to remote repo

```$git push - u origin master```

* Edit the * README.md * file as per project requirements and push the changes to remote repo.


# Set up Anaconda environment

It is good practice to create separate environment for each project. Anaconda distribution is commonly used for handling libraries in data science projects. Here, I will describe how to set up an Anaconda environment for the current project, which can be reproduced in another machine for recovering the experiment results. For more details, please refer to environment management [documentation](https: // docs.conda.io / projects / conda / en / latest / user - guide / tasks / manage - environments.html) by Anaconda.

* Create an Anaconda environment and give it a name e.g. *NLP*

```$conda create - -name NLP```

* Activate the environment

```$source activate NLP```

* Install required packages e.g.

```$conda install - c pytorch pytorch```

* Export the active environment to a new file in yml format

```$conda env export > NLP.yml```


* Some useful commands

    * Deactivate active environment ```$conda deactivate```
    * Delete environment ```$conda remove - -name NLP - -all```




* Open Jupyter Lab from the project folder and check whether the environment(NLP) is available in iPython kernel. If not, execute the following:

```$conda install jupyter```

```$conda install nb_conda```

```$conda install ipykernel```

```$python - m ipykernel install - -user - -name NLP - -display - name "Python (NLP)"```

* Check whether installed libraries can be imported e.g.

```python
import torch  # should not get errors
```

* Keep updating the yml file for conda environment when new libraries are installed

```$conda env export > NLP.yml```


* To recreate same conda environment in another machine from yml file use the following command

```$conda env create - f NLP.yml```

* Create a requirements.txt file for any non conda packages. You may use pip for installing such packages. To this end, first install pip for the environment

```$conda install -n NLP pip```

```$conda activate NLP```

```$pip <pip_subcommand>```

```$pip freeze > requirements.txt```


## Repo Structure
```
├── .gitignore
├── conda_env.yml
├── LICENSE
├── README.md
├── requirements.txt
├── setup.py
│
├── data
│   ├── interim_[desc]
│   ├── processed
│   ├── raw
│   └── temp
│
├── docs
│   └── process_documentation.md
│
├── examples
│
├── extras
│   └── add_explorer_context_shortcuts.reg
│
├── notebooks
│   ├── eda
│   │   └── example.ipynb
│   ├── features
│   ├── modelling
│   └── preprocessing
│
├── reporting
│   ├── webapp
│   └── README.md
│
├── src
│   └── examplepackage
│       ├── examplemodule.py
│       ├── features.py
│       ├── io.py
│       └── pipeline.py
│
└── tests
    ├── test_notebook.py
    ├── examplepackage
        ├── examplemodule
        ├── features
        ├── io
        └── pipeline
```
