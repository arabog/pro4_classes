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

