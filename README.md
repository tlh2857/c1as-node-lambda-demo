# c1as-node-lambda-demo

### This is a demo application that is designed to demonstrate the protective abilities of [Trend Micro Cloud One Application Security](https://cloudone.trendmicro.com/docs/application-security/) using a Node.js application hosted on AWS Lambda and API Gateway.

### Prerequisites
- An AWS account
- Serverless Framework installed and configured to your AWS account
  - Alternatively, if you don't want to use the serverless framework, you can provision the resources using another method. Note that the solution provisions an API Gateway with Lambda proxy integration. The Lambda functions needs a custom runtime and this Lambda layer ARN to be associated: `arn:aws:lambda:us-east-1:800880067056:layer:CloudOne-ApplicationSecurity-runtime-nodejs12_x:1`. 
  - The Serverless Framework requires Node.js to be installed locally. Here are the [Serverless Framework Installation Instructions](https://www.serverless.com/framework/docs/providers/aws/guide/installation/)
  - To configure the Serverless Framework with your AWS credentials, follow (these instructions)[https://www.serverless.com/framework/docs/providers/aws/guide/credentials/]. 

### Installation

1. Clone/download the repository
2. In the root directory, run `
