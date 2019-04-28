# Architecture

This is a complete serverless application which created using five managed services:
 - API Gateway
 - DynamoDB
 - Lambda
 - Simple Email Service
 - CloudFormation

## API Gateway

API Gateway used as a proxy for DynamoDB table, and also for managing access to the application using an apikey.

## DynamoDB

DynamoDb used for storing and streaming messages. 

## Lambda

Lambda function listens on DynamoDB stream for updates, when a new message detected the function uses Simple Email Service to send message to a user, then updates status for that message in DynamoDB.

The source code of the lambda function is inside the CloudFormation template.

## Simple Email Service

Simple Email Service is required a few additional manual steps before usage:
 - setup a domain
 - verify an email address or request to move account out of the sandbox

## CloudFormation

CloudFormation uses to create all necessary part of the application.

# How to deploy

1. Verify a new domain in SES
1. Verify a new email address in SES
1. Create a new CloudFormation stack using the ```send_message.cf``` template
   - Put email address form the verified domain as a parameter 

# How to clean up
1. Delete the CloudFormation stack
1. Delete CloudWatch Log Groups
1. Delete verified email address in SES
1. Delete verified domain in SES

# How to use
## Send a message to the recipient

```
POST /<stage name>/messages/ HTTP/1.1
Host: <FQDN of depoyed api>
Content-Type: application/json
x-api-key: <API Key>

{
    "recipient": "<email address>",
    "message": "<message>"
}
```
## Retrieve the messages sent to a particular recipient

```
GET /<stage name>/messages/recipient/<URL encoded email address> HTTP/1.1
Host: <FQDN of depoyed api>
x-api-key: <API Key>
```
