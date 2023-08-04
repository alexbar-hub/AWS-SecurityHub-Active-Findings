# AWS-SecurityHub-Active-Findings
Creates a lambda that gets triggered by ACTIVE-NEW SecurityHub Findings via EventBridge. The lambda formats SecurityHub findings json code into a more readable format and notifies via SNS. It also updates notified findings so that no duplicate tickets are raised.

## Table of contents
- [AWS-SecurityHub-Active-Findings](#aws-securityhub-active-findings)
  - [Table of contents](#table-of-contents)
  - [Intro](#intro)
  - [What it does](#what-it-does)
  - [Deployment](#deployment)
  - [Credits](#credits)

## Intro
This project builds a notification system so that relevant SecurityHub findings can be sent to the appropriate team. The filtering can be done on the finding title or AWS account. This system also prevents duplicate tickets from being raised.

## What it does
This CloudFormation Stack creates the following resources:
* a set of SNS topics, ideally one for each team, so that they can get only the notifications relevant to them
* an EvenBridge rule that triggers a Lambda based on a specific pattern (finding's RecordState has to be ACTIVE and Workflow Status has to be NEW);
* an IAM role, with 3 policies, so that the Lambda can do its job;
* a Lambda function that gets triggered by the above EventBridge rule, reformats the json payload, extracts the needed information, and sends the notification to the relevant SNS topic, and, once the notification is sent, also **resets the findings in SecurityHub to RecordState=ACTIVE and WorkflowStatus=NOTIFIED so that no duplicate notifications are sent through**;

Note: an example to reset the notified finding to SUPPRESSED is hardcoded in the lambda function for guidance.


## Deployment
1. Review the lambda code as some finding titles and AWS account ID placeholders are hardcoded in it for guidance and as examples. Update them according to your needs.

2. In order to deploy this template:
   1. open CloudFormation in the account in which you have SecurityHub configured;
   2. populate all the required fields above
   3. submit

## Credits
The original Lambda function I used to build this project is available at the following: [How do I set up customized email notifications for Security Hub findings using EventBridge with an Amazon SNS topic?](https://aws.amazon.com/premiumsupport/knowledge-center/sns-email-notifications-eventbridge/).
