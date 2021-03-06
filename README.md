# c1as-node-lambda-demo

### This is a demo application that is designed to demonstrate the protective abilities of [Trend Micro Cloud One Application Security](https://cloudone.trendmicro.com/docs/application-security/) using a Node.js application hosted on AWS Lambda and API Gateway.

#### Warning: This Node.js application uses the `eval()` method to process raw user input. While Cloud One Application Security can protect against a number of attacks, it doesn't not directly protect against Remote Code Injections, and thus you need to ensure that permissions granted to this application follow least priviledges. By default, the Lambda application provisioned by the Serverless Framework has permissions to create and push to a CloudWatch Log Stream. 

### Prerequisites
- An AWS account
- Serverless Framework installed and configured to your AWS account
  - Alternatively, if you don't want to use the serverless framework, you can provision the resources using another method. Note that the solution provisions an API Gateway with Lambda proxy integration. The Lambda functions needs a custom runtime and this Lambda layer ARN to be associated: `arn:aws:lambda:us-east-1:800880067056:layer:CloudOne-ApplicationSecurity-runtime-nodejs12_x:1`. 
  - The Serverless Framework requires Node.js to be installed locally. Here are the [Serverless Framework Installation Instructions](https://www.serverless.com/framework/docs/providers/aws/guide/installation/)
  - To configure the Serverless Framework with your AWS credentials, follow [these instructions](https://www.serverless.com/framework/docs/providers/aws/guide/credentials/).
- A Cloud One account with an Application Security Security group and associated credentials
  - You can access the Cloue One console, [here](http://cloudone.trendmicro.com/). From there, either login, or click “Create an Account” and follow the steps to get a free trial of Cloud One. Once registered and logged into Cloud One, click on the “Application Security” tab, and then click “Create New Group”. This will allow you logicaly apply and isolate security settings and events with the demo Lambda application. 
  - [Here are instructions to create a Security Group in Application Security](https://cloudone.trendmicro.com/docs/application-security/groups/#view-and-modify-group-configurations)
  - [Here are instructuions to configure the policy of your security group](https://cloudone.trendmicro.com/docs/application-security/policies/)

### Installation

1. Clone/download the repository
2. In the root directory, open the file called "trend_app_protect.json" and edit the "key" and "secret" values with those of your Security group. 
   - more details on configuring the environment variables can be found in the official [Cloud One Documentation](https://cloudone.trendmicro.com/docs/application-security/nodejs-with-express/#install-the-agent). 

3. Open a command line terminal in the root directory, and run: 
```
npm install
serverless deploy
```
4. Head the API endpoint in the logs generated by running the `serverless deploy` command. It should look something like this: 
   - https://abcdefg123.execute-api.us-east-1.amazonaws.com/dev/


### Testing Different Security Modules

#### Malicious Payload
1. Paste the following scripts into the form labeled "Eval Form": 
```
POST /login HTTP/1.1
Host: vulnerable-website.com
```
2. Click on the "Submit" button.
3. Go to the Cloud One Applicaiton Security Console to view the attack event. 

#### Malicious File Upload 
1. Download a malware TEST file from [eicar](https://secure.eicar.org/eicar.com)
2. Click on 'Upload File', select the eicar test file, and then hit 'Submit'. 
3. Go to the Cloud One Applicaiton Security Console to view the attack event. 

#### Illegal File Access
1. Paste the following scripts into the form labeled "Eval Form": 
```
require('fs').readFileSync('/proc/self/environ').toString()
```
2. Click on the "Submit" button.
3. Go to the Cloud One Applicaiton Security Console to view the attack event. 
#### Open Redirect
1. Paste the following scripts into the form labeled "Eval Form": 
```
res.redirect(encodeURI('https://www.facebook.com'))
```
2. Click on the "Submit" button.
3. Go to the Cloud One Applicaiton Security Console to view the attack event. 
#### Remote Command Execution
1. Paste the following scripts into the form labeled "Eval Form": 
```
let time = new Date();
require('fs').writeFileSync(`/tmp/${time}`,`curl http://example.com && touch /tmp/new-${time}`);
const { exec } = require('child_process');
var yourscript = exec(`sh /tmp/${time}`,
        (error, stdout, stderr) => {
            console.log(stdout);
            console.log(stderr);
            if (error !== null) {
                console.log(`exec error: ${error}`);
            }
        });
```
2. Click on the "Submit" button.
3. Go to the Cloud One Applicaiton Security Console to view the attack event. 
