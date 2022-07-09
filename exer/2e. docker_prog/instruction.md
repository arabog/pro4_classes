cr8 a new repo

ssh -keygen -t rsa

config d github project with ssh key

clone:
git clone git@github.com:arabog/docker_prog.git

Or:
git clone https://github.com/arabog/docker_prog.git

touch Makefile

touch requirement.txt

touch Dockerfile

touch app.py

after adding app.py content run:
chmod +x app.py

test app.py locally
python app.py

cr8 a virtual env
python3 -m venv ~/.docker_prog

source ~/.docker_prog/bin/activate

make install

install hadolint: https://github.com/hadolint/hadolint/releases

wget -O /bin/hadolint https://github.com/hadolint/hadolint/releases/download/v2.10.0/hadolint-Linux-x86_64 && \

chmod +x /bin/hadolint


docker build --tag=app .

docker image ls

docker run -it app bash

ls

python app.py

exit

<!-- Build circleci -->
mkdir .circleci

touch .circleci/config.yml
paste ds in it:
# Python CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-python/ for more details
#
version: 2
jobs:
  build:
    docker:
      # specify the version you desire here
      # use `-browsers` prefix for selenium tests, e.g. `3.6.1-browsers`
      - image: circleci/python:3.6.1
      
      # Specify service dependencies here if necessary
      # CircleCI maintains a library of pre-built images
      # documented at https://circleci.com/docs/2.0/circleci-images/
      # - image: circleci/postgres:9.4

    working_directory: ~/repo

    steps:
      - checkout

      # Download and cache dependencies
      - restore_cache:
          keys:
          - v1-dependencies-{{ checksum "requirements.txt" }}
          # fallback to using the latest cache if no exact match is found
          - v1-dependencies-

      - run:
          name: install dependencies
          command: |
            python3 -m venv venv
            . venv/bin/activate
            pip install -r requirements.txt
      - save_cache:
          paths:
            - ./venv
          key: v1-dependencies-{{ checksum "requirements.txt" }}
        
      # run tests!
      - run:
          name: run tests
          command: |
            . venv/bin/activate
            make test
      # run lints!
      - run:
          name: run lint
          command: |
            . venv/bin/activate
            make lint


search for circleci local and install
(.docker_prog) voclabs:/tmp $ 
cd /tmp

https://github.com/CircleCI-Public/circleci-cli/releases
wget curl https://github.com/CircleCI-Public/circleci-cli/releases/download/v0.1.19666/circleci-cli_0.1.19666_linux_amd64.tar.gz

tar zxvf circ...

cd circleci-cli_0.1.19666_linux_amd64

move to home directory
mv circleci ~/

(.docker_prog) voclabs:~ $ ls

mv circleci environment

(.docker_prog) voclabs:~/environment $ ls

cd docker_prog

pwd

<!-- voclabs:~/environment $ mv circleci docker_prog/ -->
mv circleci docker_prog

move circleci back to /tmp
mv circleci /tmp

<!-- voclabs:~/environment/docker_prog (main) $ git status -->
git status

git add .
git add *


https://www.containiq.com/post/hadolint

