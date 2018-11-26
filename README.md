## serverless-cognito-auth app

This builds on the [serverless-stack demo](https://serverless-stack.com) to create the backend for a simple web auth experience with AWS Cognito.

## Usage
Skip steps as necessary if you already have certain aspects configured (e.g. AWS IAM user/install serverless etc.)
* Install serverless globally - npm install serverless -g
* Clone repo
* cd to repo directory
* `npm install`
* [Create AWS IAM user](https://serverless-stack.com/chapters/create-an-iam-user.html)
* [Install AWS CLI](https://serverless-stack.com/chapters/configure-the-aws-cli.html)
* Login to the console/terminal
* `sls deploy -v`
  * sls = shorthand for serverless
  * -v = verbose - need to see the outputs at the end of the build to configure the front end
* On success, copy down the Stack Outputs
* Test 
  * Replace all caps variables with corresponding stack outputs:
	  * COGNITO_REGION = region in UserPoolId
	  * ENDPOINT_REGION = region in ServiceEndpoint url
  * Signup
    ```bash
    aws cognito-idp sign-up \
      --region COGNITO_REGION \
      --client-id USERPOOLCLIENTID \
      --username example@example.com \
      --password SuperSecretP@ssword1
    ```
  * Confirm signup
	  ```bash
    aws cognito-idp admin-confirm-sign-up \
      --region COGNITO_REGION \
      --user-pool-id USERPOOLID \
      --username example@example.com
      ```
  * Lambda function
	  ```bash
    npx aws-api-gateway-cli-test \
      --username='example@example.com' \
      --password='SuperSecretP@ssword1' \
      --user-pool-id='USERPOOLID' \
      --app-client-id='USERPOOLCLIENTID' \
      --cognito-region='COGNITO_REGION' \
      --identity-pool-id='IDENTITYPOOLID' \
      --invoke-url='SERVICEENDPOINT' \
      --api-gateway-region='ENDPOINT_REGION' \
      --path-template='/notes' \
      --method='POST' \
      --body='{"content":"hello world","attachment":"hello.jpg"}'
      ```

