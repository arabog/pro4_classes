Linting and CircleCI
Extending a Makefile for use with Docker Containers
Beyond the simple Makefile, it is also useful to extend it to do other things. An example of this is as follows:

Example Makefile for Docker and CircleCI

https://docs.docker.com/desktop/linux/install/ubuntu/

sudo snap install docker circleci
sudo snap connect circleci:docker docker

https://circleci.com/docs/local-cli#linux-install-with-snap

https://tcoil.info/how-to-install-hadolint-on-linux/

sudo wget -O /bin/hadolint https://github.com/hadolint/hadolint/releases/download/v1.16.3/hadolint-Linux-x86_64

sudo chmod +x /bin/hadolint

/bin/hadolint Dockerfile

https://github.com/udacity/DevOps_Microservices/tree/master/Lesson-2-Docker-format-containers/class-demos

/Desktop/local_env/DevOps_Microservices/Lesson-2-Docker-format-containers/class-demos/demos/flask-sklearn-student-starter


nano Makefile

https://github.com/udacity/DevOps_Microservices/tree/master/Lesson-2-Docker-format-containers/class-demos

/bin/hadolint Dockerfile

Desktop/local_env/DevOps_Microservices$ ls -la

Desktop/local_env/DevOps_Microservices/Lesson-2-Docker-format-containers/class-demos/.circleci$ nano config.yml 

if run:
 Desktop/local_env/DevOps_Microservices/Lesson-2-Docker-format-containers/class-demos$ make [space + tap]
 it gives u oda options

 Desktop/local_env/DevOps_Microservices/Lesson-2-Docker-format-containers/class-demos$ make run-circleci-local 

You can also use operating system utilities, such as sudo systemctl is-active docker or sudo status docker

