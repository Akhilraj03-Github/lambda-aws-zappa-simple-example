# Lambda AWS Zappa Simple Example with Python

![](https://img.shields.io/badge/Python-3.6-blue.svg)

This is a simple example to deploy a Python code in AWS Lambda

This code will return a simple "Hello World" in console.


### Environment

* Ubuntu 16.04
* Python 3.6.3
* [VirtualEnv Wrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)
* [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)


### Configure the AWS Credentials

AWS Credentials are necessary to connect in your account in AWS.

First of all, install the aws-cli package using pip

```
pip install awscli --upgrade --user
```

Run the aws configure command to configure the AWS CLI with your Amazon account:

```
aws configure
```

Put your credentials KEY and SECRET:
```
Aws Access Key ID [************ABC]:
AWS Secret Access Key [***********XYZ]:
Default region name [us-east-1]:
Default output format [json]:
```


### Virtual Env (venv)

Creating a virtual env to your project using Virtual Env Wrapper

```
mkvirtualenv --python=/usr/bin/python3.6 lambda-aws-zappa-simple-example
```

This indicate that your venv is active to use:

```
(lambda-aws-zappa-simple-example) ph@ph-note: ~/your-folder/project$
```

### Pip install using requirements.txt

Note: Zappa doesn't work in the last version of PIP, at this time was 18.0.

```
pip install -r requirements.txt
```

### The code .py

The is a simple "Hello world"

```

def lambda_handler(event, context):
    
    return 'Hello Word'

```

### Create Zappa Settings file

Zappa settings file is the data used by zappa to deploy the application in Lambda.

To initiate, execute this code and put the information step-by-step:
```
zappa init
```

Zappa ask the name of your environment. You can create how many environment you need. In this step we put 'dev'
```
What do you want to call this environment (default 'dev'):
```

Zappa found profiles in your AWS account, you can put the name which it founds or default name. We put 'default'
```
We found the following profiles: default, and xpto. Which would you like us to use? (default 'default'):
```

Zappa use S3 to upload your project and then put the code in Lambda. For this, it asks you the bucket's name that the project will use.
This we will use the name that it suggests.
Note: the KEY used in aws configure must have right access in S3.

```
Your Zappa deployments will need to be uploaded to a private S3 bucket.
What do you want call your bucket? (default 'zappa-sbjfdpufh'): 
```

The AWS Lambda function requires an attribute, such as lambda_handler , which points
to a function as an entry point for Lambda execution. Hence, we need to provide
information about the function name with a modular path such as
<filename>.<function_name/app_name> to Zappa.

In this case our file called lambda_function.py and our function is called lambda_handler.
So, in this step we put:  lambda_function.lambda_handler

```
What's the modular path to your app's function?
```

AWS provides a feature to extend the Lambda services to all available regions. If you want to make your service globally with less latency, put y in this step.
In this step we put 'n' 
```
You can optionally deploy to all available regions in order to provide fast global service
```

Zappa creates a file called zappa_settings.json, like this:
```
{
    "dev": {
        "app_function": "lambda_function.lambda_handler",
        "aws_region": "sa-east-1",
        "profile_name": "default",
        "project_name": "lambda-aws-zapp",
        "runtime": "python3.6",
        "s3_bucket": "zappa-x5woukxo7"
    }
}

```

### Customize other options in Zappa settings

Zappa has a lot of possibilities to customize your project. In this project what we will do:

* not use Api Gateway
* memory size is 128mb
* write a description
* use a AWS Role
* timeout 10 seconds

For this, edit the zappa_settings.json to be like this:

```
{
    "dev": {
        "apigateway_enabled": false,
        "app_function": "lambda_function.lambda_handler",
        "aws_region": "sa-east-1",
        "keep_warm": false,
        "lambda_description": "Simple Example",
        "lambda_handler": "lambda_function.lambda_handler",
        "memory_size": 128,
        "profile_name": "default",
        "project_name": "lambda-aws-zappa-simple-example",
        "role_name": "ApiLambda",
        "runtime": "python3.6",
        "s3_bucket": "zappa-a1sgi38si",
        "timeout_seconds": 10
    }
}

```

### Deploy the app

```
zappa deploy dev
```

### How to execute lambda function using a file.py

To execute your lambda function in a Python script we will use the SDK [Boto 3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)


Create a venv and install boto3

```
pip install boto3
```

Script:

```
import boto3
import json

AWS_ACESS_KEY_ID = '<you_access_key>'
AWS_SECRET_ACESS_KEY = '<your_secret>'

client = boto3.client(
    'lambda',
    region_name='sa-east-1',
    aws_access_key_id=AWS_ACESS_KEY_ID,
    aws_secret_access_key=AWS_SECRET_ACESS_KEY
)

invoke_response = client.invoke(
    FunctionName='lambda-aws-zappa-simple-example-dev',
    InvocationType='RequestResponse',
    Payload=payload
)

response = json.loads(invoke_response['Payload'].read())

print(response)


```

```
(venv)ph@ph-note:~/your-folder$ python your_python_file.py
```

```
Hello World!
```

That's it !


### Fonts

* Building Serveless Python Web Services with Zappa - Barguzar, Abdulwahid Abdulhaque - Packt - 2018 - ISBN 978-1-78883-761-3  
