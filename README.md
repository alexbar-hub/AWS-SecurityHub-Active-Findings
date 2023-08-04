# AWS-SecurityHub-Active-Findings
Creates a lambda that gets triggered by certain SecurityHub Findings via EventBridge. The lambda formats SecurityHub findings json code into a more readable format and notifies via SNS. It also updates notified findings so that no duplicate tickets are raised.
