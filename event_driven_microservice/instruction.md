-: Tutorial: Making Change
Prerequisites
You should have a Cloud9 environment running already in a separate browser tab.
You must have cloned the Github repo in the Cloud9 environment and navigate to the folder contaning the exercise code.

# Clone the repo if not already
git clone https://github.com/[Your-username]/DevOps_Microservices.git

# Navigate to the folder contaning the exercise code
cd DevOps_Microservices/Lesson-1-Lambda-functions/make-change-tutorial


Exercise Overview
Now, you're ready to deploy a more complex Function as a Service (FaaS). The next interactive tutorial will be about event-handling for a function that automatically makes the smallest amount of change given an input amount of US currency. For example, given $0.63, the function should output: 2 quarters, 1 dime, 3 pennies.

Instructions
Create a new AWS Lambda function or use an existing function in the Cloud9 environment. Assuming the app name is HelloWorldLambda, double-click to open the HelloWorldLambda/hello_world/app.py file.

Update requirement.txt - Edit the HelloWorldLambda/hello_world/requirement.txt file to include the depdendencies needed by the new application.
requests
In the next exercise you will add newer dependency - wikipedia.
Create a virtual environment and install dependencies
A virtual environment is an isolated Python environment having its own set of packages installed.

cd [Application-name]/hello_world 
python3 -m venv ~/.myvenv
source ~/.myvenv/bin/activate
python3 -m pip install -r requirements.txt

Create payload.json file - Write a test payload (JSON), passing in some $ amount as a request to your function, and view the JSON response. The payload needs to be JSON formatted as
{    
   "amount":1.45
}
Once you get that working (later), feel free to try new values by putting different amounts into the JSON payload. For example, {"amount":1.50}.

$ HelloWorldLambda: touch payload.json

Run the application locally in the Cloud9 environment:
# Assuming you have updated the app.py, requirement.txt, 
# Assumnig you have created the  [Application-name]/payload.json file

cd [Lambda-function-name]
% DevOps_Microservices/HelloWorldLambda/hello_world (master) $ sam build

sam local invoke -e payload.json

# If you were doing this development in your physical local machine and want to test more, use: 
sam local start-api

# It will generate an API endpoint tha you can curl in s diiffernt terminal window. 

Deploy the app - Deploy the newly created and tested lambda function.
# Use the URI of the new ECR repo, if created manually
sam deploy --guided

# Go to AWS ECR and create a new repo to store the newly created custom image. 
After deployment, you'd see the Lambda function in the AWS explorer panel. You can also test it out in the AWS Lambda web console and make the function return the correct change.

