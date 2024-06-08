## About Project

I have created a serverless web application that enables users to request unicorn rides from the Wild Rydes fleet via [this tutorial](https://aws.amazon.com/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/?ref=gsrchandson). The application will present users with an HTML-based user interface for indicating a pick-up location and a RESTful web service on the backend to submit the request for dispatching a unicorn. The application will also provide facilities for users to register with the service and log in before requesting rides.

## Application Architecture

The application architecture uses AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

![resim](https://github.com/emirhan-mete/wildrydes-site/assets/41618474/a0d87d13-5bca-45ac-a7a9-607854a47967)


## Issues I Faced

After setting up the Cognito User Pool, I received an this error when trying to register as a user with an e-mail address:
<pre>InvalidParameterException: Username should be an email</pre>

  #### Solution
I reviewed the sample code provided by AWS and in the js/cognito-auth.js file I found that where the username parameter is being passed to Cognito it is wrapped in a toUsername() function:


<pre>
userPool.signUp(toUsername(email), password, [attributeEmail], null)
</pre>

The function replaces the @ in the email with -at-:

<pre>
function toUsername(email) {
  return email.replace('@', '-at-');
}
</pre>

I updated the toUsername() function:

<pre>
function toUsername(email) {
    return email;
}
</pre>
