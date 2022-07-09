Docker Containers
Getting started with Docker
There are two main components of Docker: Docker Desktop and Docker Hub.

https://www.docker.com/products/docker-desktop/

https://www.docker.com/products/docker-hub/


Docker Desktop Overview
The desktop application contains the container runtime which allows containers to execute. The Docker App itself orchestrates the local development workflow including the ability to use Kubernetes, which is an open-source system for managing containerized applications that came out of Google.

https://github.com/arabog/kubernetes


Docker Hub Overview
So what is Docker Hub and what problem does it solve? Just as the git source code ecosystem has local developer tools like vim, emacs, Visual Studio Code or XCode that work with it, Docker Desktop works with Docker containers and allows for local use and development.

When collaborating with git outside of the local environment, developers often use platforms like Github or Gitlab to communicate with other parties and share code. Docker Hub works in a similar way. Docker Hub allows developers to share docker containers that can serve as a base image for building new solutions.

These base images can be built by experts and certified to be high quality: i.e. the official Python developers have a base image. This allows a developer to leverage the expertise of the true expert on a particular software component and improve the overall quality of their container. This is a similar concept to using a library developed by another developer versus writing it yourself.
https://hub.docker.com/_/python


Real-World Examples of Containers
What problem do Docker format containers solve? In a nutshell, the operating system runtime can be packaged along with the code, and this solves a particularly complicated problem with a long history. There is a famous meme that goes "It works on my machine!". While this is often told as a joke to illustrate the complexity of deploying software, it is also true. Containers solve this exact problem. If the code works in a container, then the container configuration can be checked in as code. Another way to describe this concept is that the actual Infrastructure is treated as code. This is called IaC (Infrastructure as Code).

Here are a few specific examples:

Developer Shares Local Project
A developer can work on a web application that uses flask (a popular Python web framework). The installation and configuration of the underlying operating system is handled by the Docker container file. Another team member can checkout the code and use docker run to run the project. This eliminates what could be a multi-day problem of configuring a laptop correctly to run a software project.

Data Scientist shares Jupyter Notebook with a Researcher at another University
A data scientist working with jupyter style notebooks wants to share a complex data science project that has multiple dependencies on C, Fortran, R, and Python code. They package up the runtime as a Docker container and eliminate the back and forth over several weeks that occurs when sharing a project like this.

A Machine Learning Engineer Load Tests a Production Machine Learning Model
A Machine learning engineer has been tasked with taking a new model and deploying it to production. Previously, they were concerned about how to accurately test the accuracy of the new model before committing to it. The model recommends products to paying customers and, if it is inaccurate, it costs the company a lot of money. Using containers, it is possible to deploy the model to a fraction of the customers, only 10%, and if there are problems, it can be quickly reverted. If the model performs well, it can quickly replace the existing models.

Why Docker Containers vs Virtual Machines?
What is the difference between a container and a virtual machine? Here is a breakdown:

Size: Containers are much smaller than Virtual Machines (VM) and run as isolated processes versus virtualized hardware. VMs can be GBs while containers can be MBs.

Speed: Virtual Machines can be slow to boot and take minutes to launch. A container can spawn much more quickly typically in seconds.

Composability: Containers are designed to be programmatically built and are defined as source code in an Infrastructure as Code project (IaC). Virtual Machines are often replicas of a manually built system. Containers make IaC workflows possible because they are defined as a file and checked into source control alongside the project’s source code.

-: Makefiles

Setup Makefile
Just like vim, mastering Makefiles can take years, but a minimalistic approach provides immediate benefits. A main benefit to a Makefile is the ability to enforce a convention. If everytime you work a project you follow a few simple steps, it reduces the possibility of errors in building and testing a project.

A typical Python project can be improved by adding a Makefile with the following steps: make setup, make install, make test, make lint and make all.

setup:
    python3 -m venv ~/.myrepo

install:
    pip install --upgrade pip &&\
        pip install -r requirements.txt

test:
    python -m pytest -vv --cov=myrepolib tests/*.py
    python -m pytest --nbval notebook.ipynb


lint:
    pylint --disable=R,C myrepolib cli web

all: install lint test

This example is from a tutorial repository called myrepo. There is also an article about how to use it from CircleCI.

https://github.com/noahgift/myrepo

https://circleci.com/blog/increase-reliability-in-data-science-and-machine-learning-projects-with-circleci/

The general idea is that a convention eliminates the need to think about what to do. For every project, there is a common way to install software, a common way to test software and a common way to test and lint software. Just like vim, a Makefile build system is often already on a Unix or Linux system. Even Microsoft uses the Linux operating system in Azure, and the result is that Linux is the preferred deployment target for most software.

Noah's Makefile
Here is what is contained in Noah's Makefile:

setup:
    python3 -m venv ~/.myrepo

install:
    pip install -r requirements.txt

test:
    python -m pytest -vv --cov=myrepolib tests/*.py
    python -m pytest --nbval notebook.ipynb

lint:
    pylint --disable=R,C myrepolib cli web

all: install lint test

Note that each line is indented with a tab.

Noah's requirements.txt
Below are the libraries included in Noah's requirements.txt file:

pytest
pylint
jupyter
pytest-cov
pandas
nbval
click
flask
requests

Makefile Creation Recap
Let’s recap the key concepts of creating a Makefile.

setup: You have seen most of this line before, which is dealing with our Python 3 virtual environment.

install: This installs the requirements for our environment. In our case, it also install the pytest and pylint libraries used later on in the Makefile.

test: This is broken into two parts for testing.

    First, it will use .py files in the tests directory. The -vv flag ensures short test durations are still shown (see documentation), while the -cov flag helps to calculate what the test coverage of the code is (see documentation) in a given directory.
    https://docs.pytest.org/en/latest/how-to/usage.html#profiling-test-execution-duration

    The second line is used to test Jupyter Notebook cells. The --nbval flag makes pytest pay attention to jupyter notebooks (see documentation).

    https://nbval.readthedocs.io/en/latest/

lint: This will lint what is in the myrepolib directory, as well as the cli.py and web.py files in our current directory (see video). The --disable=R,C is used to disable the "convention" (C) and "refactor" (R) message classes (see related Stack Overflow post).
https://stackoverflow.com/questions/31907762/pylint-to-show-only-warnings-and-errors

https://pypi.org/project/pytest-cov/

all: You may notice this line looks a little different than the above lines, with the commands on the same line. This will execute our install, lint and test commands.

Makefile Usage Recap
Let’s recap the key concepts of a using a Makefile.
Once the Makefile is created, you can use:

make install
make lint
to install dependencies and test your code.

Q: What is Makefile's purpose?
Test software
Install software
Build executables or scripts
Collect and run tasks

Additional References
More on Makefiles
https://www.gnu.org/software/make/manual/html_node/Standard-Targets.html#Standard-Targets

Makefile tutorial
https://cs.colby.edu/maxwell/courses/tutorials/maketutor/


Desktop/local_env$  git clone https://github.com/arabog/myrepo-1

cd myrepo-1

ls -l or ls

open Makefile:
vim/nano Makefile

nano requirement.txt

to run:
make install

make lint

nano cli.py

try and alter/add smth to cii.py and run:
nano cli.py

to run test:
make test


-: Install Docker
Note: You will need to open Docker from your Applications for the first time for it to appear in the menu bar. Kubernetes may not yet be running for you, but we will come back to it later.

A few useful links for your reference:
Docker Homepage
https://www.docker.com/

Play with Docker Classroom
https://training.play-with-docker.com/

Docker Orientation and Setup
https://docs.docker.com/get-started/


-: Linting and CircleCI
Extending a Makefile for use with Docker Containers
Beyond the simple Makefile, it is also useful to extend it to do other things. An example of this is as follows:

Example Makefile for Docker and CircleCI
setup:
    python3 -m venv ~/.container-revolution-devops

install:
    pip install --upgrade pip &&\
        pip install -r requirements.txt

test:
    #python -m pytest -vv --cov=myrepolib tests/*.py
    #python -m pytest --nbval notebook.ipynb

validate-circleci:
    # See https://circleci.com/docs/2.0/local-cli/#processing-a-config
    circleci config process .circleci/config.yml

run-circleci-local:
    # See https://circleci.com/docs/2.0/local-cli/#running-a-job
    circleci local execute

lint:
    hadolint demos/flask-sklearn/Dockerfile
    pylint --disable=R,C,W1203,W1202 demos/**/**.py

all: install lint test


A Dockerfile linter is called hadolint checks for bugs in a Dockerfile. A local version of the CircleCI build system allows for testing in the same environment as the SaaS offering. The minimalist approach is still present. A user only needs to remember to use the same commands: make install, make lint and make test, but the lint step is more complete and powerful with the inclusion of Dockerfile as well as Python linting.

https://circleci.com/docs/local-cli

Notes about installing hadolint and circleci: If you are on OS X you can brew install hadolint. If you are on another platform follow the instructions from the hadolint GitHub repo. To install the local version of circleci on OS X or Linux you can run curl -fLSs https://circle.ci/cli | bash or follow the official instructions for local version of the CircleCI build system.

https://github.com/hadolint/hadolint/

https://circleci.com/docs/local-cli


CircleCI lint workflow
You can find Noah's code here. Note that the Makefile used in the video is the one in the class-demos directory, while he changes into the subdirectory within demos after that.

https://github.com/udacity/DevOps_Microservices/tree/master/Lesson-2-Docker-format-containers/class-demos


Using CircleCI
Here are the steps Noah took in the above video:

Added to the Makefile:

validate-circleci:
    circleci config process .circleci/config.yml

run-circleci-local:
    circleci local execute

lint: # This line should already be there with regular pylint
    hadolint path/to/Dockerfile

Runs hadolint Dockerfile

Uses the config.yml file within a .circleci directory

In the parent directory, runs make run-circleci-local to simulate what will happen in the remote CircleCI environment

Uses the CircleCI website (a related blog post is linked below) to test remotely

Notes about how to run this example
These instructions work the best on a Linux or OS X system or inside a Docker container itself. One way to dramatically simplify installation and configuration in the Cloud is to use a Cloud based development environment like AWS Cloud9. This was shown in the AWS Lambda examples in Lesson 1. This allows you to eliminate a huge portion of the problems you can run into when installing software. If you are expert at installing software on Linux, Windows or OS X, then this may not matter. If you want the easiest path to running these commands, use AWS Cloud9.

Reference
Increase reliability in data science and machine learning projects with CircleCI
https://circleci.com/blog/increase-reliability-in-data-science-and-machine-learning-projects-with-circleci/

AWS Cloud9
https://aws.amazon.com/cloud9/

Q: What does hadolint do?
hadolint is used to lint a Dockerfile's syntax.

-: Running Dockerfiles
Using "base" images
One of the advantages of the Docker workflow for developers is the ability to use certified containers from the "official" development teams. In this diagram a developer uses the official Python base image which is developed by the core Python developers. This is accomplished by the FROM statement which loads in a previously created container image.

As the developer makes changes to the Dockerfile, they test locally, then push the changes to a private Docker Hub repo. After this, the changes can be used by a deployment process to a Cloud or by another developer.

You can find Noah's code here, within the demos subdirectory. Note that he has begun filling out the Dockerfile and run_docker.sh files in the video.

https://github.com/udacity/DevOps_Microservices/tree/master/Lesson-2-Docker-format-containers/class-demos

Docker Cheat Sheet
Use docker image ls to see all of your created Docker images
docker run -it {image name} bash ran Noah's Docker image
Here is a quick Docker cheat sheet for your reference.

Noah's Dockerfile
Below is Noah's Dockerfile for your reference:

FROM python:3.7.3-stretch

# Working Directory
WORKDIR /app

# Copy source code to working directory
COPY . app.py /app/

# Install packages from requirements.txt
# hadolint ignore=DL3013
RUN pip install --upgrade pip &&\
    pip install --trusted-host pypi.python.org -r requirements.txt

Q: What is FROM in a Dockerfile?
FROM is a directive to load code

-: Summary
Key Terms:
Container
A container is a set of processes that are isolated from the rest of the operating system. They are often megabytes in size.

Virtual Machine
A virtual machine is the emulation of a physical operating system. They can be Gigabytes in size.

Docker Format Container
There are several formats for containers. An emerging form is Docker, which involves the definition of a Dockerfile.

pip
The pip tool installs Python packages.

pylint
The pylint tool checks the Python source code for syntax errors.

black
The black tool formats the text of Python source code automatically.

pytest
The pytest tool is a framework for running tests on Python source code.

IPython
The ipython interpreter is an interactive terminal for Python. It is the core of the Jupyter notebook.

Makefile
A Makefile is a file that contains a set of directives used to build software. Most Unix and Linux operating systems have built-in support for this file format.

CircleCI
A popular SaaS (Software as a Service) build systems used in DevOps workflows.

Docker
Docker is a company that creates container technology, including an execution engine, collaboration platform via DockerHub and a container format called Dockerfile.

Amazon ECR
Amazon ECR is a container registry that stores Docker format containers.

Source: https://noahgift.github.io/cloud-data-analysis-at-scale/topics/key-terms