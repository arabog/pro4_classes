Exercise: CircleCI
It is easy to look at build servers as a nice to have when they are a must-have for DevOps best practices. One ubiquitous cloud-based build server is CircleCI. In this exercise, you will incorporate many of the concepts from this course: Docker, Makefiles, and build systems.

Instructions
Create and use an AWS Cloud9 environment. (We also have a video tutorial in a previous lesson.)
Change the template below and add to your Makefile (this template is similar to what we used in earlier lessons).
Install hadolint by downloading it here.
Run make lint and make sure you are able to lint both Python code and a Dockerfile
Optional Setup CircleCI integration. Remember you can also refer back to the Docker setup screencast in Lesson 4 to see more ideas on hadolint.
Makefile template
setup:
    python3 -m venv ~/.udacity-devops

install:
    pip install --upgrade pip &&\
        pip install -r requirements.txt

test:
    #python -m pytest -vv --cov=myrepolib tests/*.py
    #python -m pytest --nbval notebook.ipynb


lint:
    hadolint demos/flask-sklearn/Dockerfile
    pylint --disable=R,C,W1203 demos/**/**.py

all: install lint test