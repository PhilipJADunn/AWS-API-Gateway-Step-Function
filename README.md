# AWS-API-Gateway-Step-Function

Call an API Gateway API in a state machine workflow

First we are going to create a Lambda function:

![1  Lambda creation](https://user-images.githubusercontent.com/68379635/102606139-34777380-411e-11eb-9e6d-a918fb87a0e4.PNG)

We simply want to change the body of the default code the Lambda function is created with to 'Step function has worked!':

![10  Lambda code](https://user-images.githubusercontent.com/68379635/102606805-41489700-411f-11eb-99e8-d2934bc5f988.PNG)

Next we are going to create a REST API to call our Lambda function, it will be a new API and once it is created we want to go to Actions > Create Method and select POST.
Integration type will be Lambda function, you can use proxy integration if you wish as we do not need to modify incoming request from the client:

![2  API creation](https://user-images.githubusercontent.com/68379635/102607181-bddb7580-411f-11eb-8699-70c18aaa062e.PNG)

When you click save we will get a warning that we are going to allow API Gateway permission to call our Lambda function, click OK.

We then need to deploy our API:

![3  API Deploy](https://user-images.githubusercontent.com/68379635/102608058-33941100-4121-11eb-904e-7132afca0fa5.jpg)

This will then ask us to deploy our API to a stage and I deployed mine to TEST, we then click deploy and we get a URL where we can invoke our API:

![4  API Address](https://user-images.githubusercontent.com/68379635/102608878-802c1c00-4122-11eb-822b-aa0a4741d429.PNG)

Now we want to go to Step Functions and we want to click Create state machine and we will Author with code snippets, in the definition section we want to use the JSON attached.
You will need to change the ApiEndpoint value to that of your API URL:

![5  JSON Step Function](https://user-images.githubusercontent.com/68379635/102609726-e8c7c880-4123-11eb-8a98-f90a99ea8581.PNG)

As you can see the method is POST and we are using NO_AUTH for authentication, the stage will be passed by the input parameter to the workflow, as will the Payload.

We then want to name our state machine and create a new IAM role for it:

![6  State Machine creation](https://user-images.githubusercontent.com/68379635/102610092-83280c00-4124-11eb-8089-0f309a13e794.PNG)

On our state machine we can then execute our Step Function by clicking Start Execution and for the input we put the below:

![7  Start Execution](https://user-images.githubusercontent.com/68379635/102610574-5a544680-4125-11eb-844b-ba82fdbbe4ed.PNG)

We then start the execution and this will execute successfully and we can see the execution event in the history at the bottom:

![8  Execution Event](https://user-images.githubusercontent.com/68379635/102610936-f5e5b700-4125-11eb-8be8-5bc93efa2cc0.PNG)

We can also click on CallAPI in the Graph Inspector and then in the Step Output we can see the response from our API and Lambda:

![9  Step output](https://user-images.githubusercontent.com/68379635/102611125-42c98d80-4126-11eb-8dca-3b1672e2b57e.PNG)

You can also test this in an API application such as Insomnia by entering the API URL and the POST method and this will return the body of the Lambda function:

![11  Insomnia](https://user-images.githubusercontent.com/68379635/102614627-77404800-412c-11eb-8f86-0e3cefd72780.PNG)

Once you are done I would suggest deleting your state machine as this is not within the free tier.
