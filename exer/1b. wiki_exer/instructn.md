Exercise Overview
Now, you're ready to complete some code of your own and deploy a FaaS that reads information from a Wikipedia page. For this exercise, you can copy the code directly from the files available in this Github directory

New Instructions
Create a new Lambda function HelloWiki or wikipedia-2021 using the sam init command. Choose the following options in the prompts that appear next:

Prompt	                        Recommended Response
Template source	            AWS quick start template
Package type	            Image artifact
Base image	                amazon/python3.8-base
Project name	        Your choice of the [Application-name]
AWS quick start application template	    Hello world Lambda image example

Notice the directory for the newly initiated Lambda function.

Update app.py - Copy the code present in the wikipedia_lambda_exercise.py file and paste it into the HelloWiki/hello_world/app.py file. There are some simple ToDo items for you in the wikipedia_lambda_exercise.py file. If you need help, you can refer/copy the wikipedia_lambda_solution.py code.

Update requirement.txt - Edit the HelloWiki/hello_world/requirement.txt file to include the depdendencies needed by the new application.
requests
wikipedia

Create a virtual environment and install dependencies

cd HelloWiki/hello world 
python3 -m venv ~/.wiki
source ~/.wiki/bin/activate
python3 -m pip install -r requirements.txt

Create payload.json file - Create a test payload (JSON) in the HelloWiki/ Lambda function directory, that will pass an entity as a request to your function.
{    
  "entity":"twitter"
}

DevOps_Microservices/wiki/wikipedia-2022 (master) $ touch payload.json

Once you get that working (later), feel free to try new values such as, Python_(programming_language).

Build the application
# Check the HelloWiki/README file instructions
# Assuming you have updated the app.py, and requirement.txt 
# Assumnig you have created the  HelloWiki/payload.json file
# Ensure that you are in the HelloWiki/ directory
sam build
<!-- inside where u have template.yaml -->
DevOps_Microservices/wiki/wikipedia-2022 (master) $ sam build

The command above will Dockerize the Lambda function in the Cloud9 environment. You can view the [Application-name]/hello world/app.py file to see the expected output when you'll run the app.

Run the application locally in the Cloud9 environment:
# Ensure that you are in the HelloWiki/ directory
sam local invoke -e payload.json

Further, you can deploy the app to the ECR image repository. After deployment, you'd see the Lambda function in the AWS explorer panel.

Optional Challenge
You would notice that the wikipedia package may give you results for the the ambiguous keywords, such as amazon. Too resolve this issue can you update the app.py to utilize the Wikipedia-API package? Refer to this README for example usage.

https://github.com/martin-majlis/Wikipedia-API/blob/master/README.rst

Installation
This package requires at least Python 3.4 to install because it's using IntEnum.

pip3 install wikipedia-api