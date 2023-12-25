# ☁️ AWS Project - AWS VPC With Public And Private Subnets

## 📝 Lab Overview

In this lab we will create a VPC from scratch in the AWS console using services/components like subnets, EC2 Instances, internet gateways, NAT gateways. We also will test the functionality of our infrastructure once built by SSH using EC2 Connect.


## 📐 VPC Architecture Design

![Screenshot](/img/lab_design.png)


## 👉 Step 1 - Create the VPC

* Open the VPC console
* Click on "Your VPCs" on the left-hand panel
* Click "Create VPC" button in the top right hand corner
* Name the VPC "DemoVPC"
* Set the IPv4 CIDR block to 10.0.0.0/16 (that's 65,536 IP Addresses we are assigning to our VPC)
* Leave everything the same and click "VPC" at the bottom right hand corner. DONE


![Screenshot](/img/create_vpc.png)


## 👉 Step 2 - Create the subnets

As shown on our architectural diagram we're going to create 4 Subnets :

* 1 Public subnet and 1 Private subnet in Availability Zone A
* 1 Public subnet and 1 Private Subnet in availability Zone B

Navigate to the VPC console, click on the left hand panel and click subnet.

💡 PRO TIP: Filter your VPC console with the "DemoVPC" we just created. This hides components already created for your default VPC. Shown in screenshot below.




Click on add integration, the drop down lets us specify what we want to do, we want to select
lambda for our integration since we're going to create a lambda function now it's asking us for some of the
details of that lambda function it's asking us for the aws region that it's located in and the name of that lambda function.

But before that let's create a lambda function first then we will come back to select the lambda function


![Screenshot](/img/create_api.png)


Go to the search bar and search for lambda, go to the top right and click on create function


![Screenshot](/img/create_lambda.png)


Author from scratch is a good option to use, we're going to leave pretty much everything default leave put in whatever name you want so mine was just called CrudAppFunction function and we are going to be using python 3.8 so make sure you pick python 3.8 leave everything else defaults and click create function


![Screenshot](/img/create_function.png)


We're going to doing do some code editing a little bit later in the console, directly in the editor if we click on `lambda_function.py` that is what is currently in this lambda function, it's just a lambda handler it's returning hello from lambda, we're going to keep for now so what i want to do now and come back to that later


![Screenshot](/img/lambda_function.png)



Go back the api gateway refresh the page to update the changes, go ahead and click on lambda functions and now you will see our lambda function popping up click on it and for api name we can pick whatever you want, i'm just going to call mine persons and click on the next button to go to the next section


![Screenshot](/img/api.png)


The next section is about routes we said that we wanted to create two different routes for this exercise one was a `GET` for `/getPerson` route and the other was a `POST` for `/createPerson`, change the method to get and the the resource path is going to be slash get person and then the target is going to be what we just integrated with which is our CrudAppFunction.

We also need to create a second route which is a post and that is going to be slash create person and it's also going to point to the same lambda function, because if you wanted to create two different functions one for each nothing wrong with that but i'm just going to do this in a way such that in my code itself i'll know whether or not get person or create person isn't being invoked so i can respond respectively so that's all we really need to do here click on next



![Screenshot](/img/route.png)



The next step is for stages if you want to use a stage that means something maybe test, beta or production depending on what you're trying to do you can go and name it whatever you want or leave it as default, i'm going to leave it as default in my case.

The second option is auto deploy and it basically just allows you to say if i change any settings for this HTTP API automatically deploy a new version to respect those settings if you don't like that or you think it's too dangerous you can disable that and then you'll have to manually deploy your API every time this is more similar to what you have in the REST version of API Gateway but i'm going to leave it on and click on next. Review everything and click on create


![Screenshot](/img/stage.png)


To test our API we're going to use a tool called Postman which is a tool that helps you make service calls or requests to endpoints and it allows you to configure a whole bunch of options along the way, we'll use Postman test both the `/getPerson` and the `/createPerson` API before we actually write our code. So back to the API Gateway and copy the `Invoke URL`



![Screenshot](/img/invoke_url.png)



Open Postman i'm going to open two tabs on the left, the one that i'm on right now is going to be for `/getPerson` and if i just test this first i just click on send we're  calling that API Gateway endpoint and then turn the lambda function whit the output `"Hello from lambda"`.

I will add some parameters so if i were creating a get person API the most logical thing that a person would pass into this API is probably something like `personId` as a Key and `personId123` as a value



![Screenshot](/img/postman_test_1.png)



Let's go ahead and set up the other API which is `/createPerson` so let's do the same thing other tab now i'm going to change it to POST now and the same kind of exercise applies we need to put some data in the the body of this request since it's a POST
click on body instead of params i'm going to change this to raw just so
that it's in JSON, let's say in this there's probably a million other ways to do this by the way so i'm probably doing to creating a person something like first name and then let's call it julien and let's say last name Muke and let's say email myemail gmail.com



![Screenshot](/img/postman_2.png)


Go back the lambda function, just early on the the goal of our next step is to figure out do i extract values
from my events because the values that get passed into the lambda function from API Gateway based on what we just did in Postman are going to be inside the event object.

Copy and paste the code to the `lambda_function.py` make sure you click deploy button


```bash
import json
import uuid

GET_RAW_PATH = "/getPerson"
CREATE_RAW_PATH = "/createPerson"

def lambda_handler(event, context):
    print(event)
    if event['rawPath'] == GET_RAW_PATH:
        print('Received getPerson request')
        personId = event['queryStringParameters']['personId']
        print("with param personId=" + personId)
        return { "firstName": "Julien " + personId, "lastName": "M", "email": "myEmail@gmail.com" }
    elif event['rawPath'] == CREATE_RAW_PATH:
        print('Received createPerson request')
        decodedBody = json.loads(event['body'])
        firstname = decodedBody['firstname']
        print('with param firstname=' + firstname)
        return { "personId": str(uuid.uuid1())}

```


![Screenshot](/img/lambda_code.png)



Let's go back to Postman and invoke this we're just going to get a response back but if we click on send we should see some kind of person id key and a random ID being returned back to us



![Screenshot](/img/postman_3.png)



Let's quickly go over to Cloudwatch take a look at the log lines just to confirm everything worked correctly, go to the law group then to the new log stream and scroll all the way down to the bottom to the most recent one, you'll see start request id there's the object start request for create person received request for with first name is julien and request



![Screenshot](/img/cloudwatch.png)


This tutorial showed you how to create a HTTP API using AWS Lambda.
