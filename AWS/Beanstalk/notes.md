Here are AWS CloudFormation Scenario-Based Interview Questions & Answers:

🟡 Beginner Scenarios
Q1. You deployed a CloudFormation stack and it failed midway. What happens?
Answer: By default, CloudFormation performs an automatic rollback to the previous stable state.

All resources created during the failed deployment get deleted
Check Stack Events tab in the console to find the exact error
You can disable rollback using --disable-rollback flag (for debugging)
Fix the template and redeploy


Q2. How do you pass environment-specific values (dev/prod) in CloudFormation?
Answer: Use Parameters in the template:
yamlParameters:
  Environment:
    Type: String
    AllowedValues: [dev, staging, prod]

Pass values at deploy time via CLI: --parameter-overrides Environment=prod
Use Conditions to create resources based on parameter values
Or use SSM Parameter Store to fetch values dynamically


Q3. You want to reuse a common template (e.g., VPC setup) across multiple stacks. How?
Answer: Use Nested Stacks:

Store the reusable template in S3
Reference it using AWS::CloudFormation::Stack resource type

yamlResources:
  VPCStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/bucket/vpc-template.yaml

Parent stack passes parameters; child stack exports outputs


🟠 Intermediate Scenarios
Q4. Your CloudFormation update is stuck in UPDATE_IN_PROGRESS. What do you do?
Answer:

Check Stack Events for the stuck resource
Common causes: Lambda custom resource not returning a response, IAM permission issue, resource waiting on external dependency
If a Custom Resource Lambda is stuck — check CloudWatch logs, ensure it sends SUCCESS/FAILED response back to the pre-signed S3 URL
Use CancelUpdateStack API if needed
Last resort: manually resolve the stuck resource and then signal the stack


Q5. How do you prevent accidental deletion of a production stack?
Answer:

Enable Termination Protection on the stack:

bashaws cloudformation update-termination-protection \
  --enable-termination-protection \
  --stack-name my-prod-stack

Set DeletionPolicy: Retain on critical resources (RDS, S3):

yamlMyDatabase:
  Type: AWS::RDS::DBInstance
  DeletionPolicy: Retain

Use Stack Policies to restrict update/delete actions on specific resources
Apply IAM policies to restrict who can delete stacks


Q6. You need to share outputs from one stack with another stack. How?
Answer: Use Cross-Stack References:
Exporting stack:
yamlOutputs:
  VPCId:
    Value: !Ref MyVPC
    Export:
      Name: prod-vpc-id
Importing stack:
yamlProperties:
  VpcId: !ImportValue prod-vpc-id

Note: You cannot delete the exporting stack while another stack imports its value
Alternative: Use SSM Parameter Store for more flexible sharing


Q7. A CloudFormation stack update is causing downtime on your EC2 instances. How do you avoid this?
Answer: Use Update Policies with rolling updates:
yamlUpdatePolicy:
  AutoScalingRollingUpdate:
    MinInstancesInService: 1
    MaxBatchSize: 1
    PauseTime: PT5M
    WaitOnResourceSignals: true

Combined with an Auto Scaling Group, this replaces instances in batches
Use cfn-signal in UserData to signal when instance is ready
Enable health checks so CloudFormation waits for healthy instances before proceeding


🔴 Advanced Scenarios
Q8. How do you deploy CloudFormation templates across 50 AWS accounts and multiple regions?
Answer: Use CloudFormation StackSets:

Define the template once, deploy to multiple accounts/regions simultaneously
Requires Administrator role in the management account and Execution role in target accounts
Use AWS Organizations integration for automatic deployment to new accounts
Control deployment order, failure tolerance, and concurrency:

bashaws cloudformation create-stack-set \
  --stack-set-name my-stackset \
  --template-url https://s3.amazonaws.com/bucket/template.yaml \
  --deployment-targets OrganizationalUnitIds=ou-xxxx

Q9. You need to run a custom script (e.g., populate a DB) during stack creation. How?
Answer: Use Custom Resources backed by Lambda:
yamlMyCustomResource:
  Type: Custom::DBSeed
  Properties:
    ServiceToken: !GetAtt SeedLambda.Arn
    DBEndpoint: !GetAtt MyDB.Endpoint.Address

CloudFormation invokes the Lambda with Create, Update, or Delete event
Lambda must send a response back (SUCCESS/FAILED) to a pre-signed S3 URL
Use for anything CloudFormation doesn't natively support


Q10. Your template is getting too large (over 1MB). How do you manage it?
Answer:

Break it into Nested Stacks — separate templates for networking, compute, database
Store templates in S3 and reference via URL (S3 template limit is 1MB per template)
Use AWS CDK or SAM which synthesize modular templates
Move repeated configurations to CloudFormation Modules (reusable template fragments registered in CloudFormation registry)
Use Mappings instead of hardcoding repeated values


Q11. How do you handle a situation where a CloudFormation resource already exists in the account?
Answer: CloudFormation will throw an "Already Exists" error.
Options:

Import existing resources using CloudFormation Resource Import:

bashaws cloudformation create-change-set \
  --change-set-type IMPORT \
  --resources-to-import file://import.json

Define the resource in the template and provide its physical ID during import
After import, CloudFormation manages the resource going forward
If you can't import, delete the existing resource first (risky in prod)


Q12. How do you do a safe, reviewable update to a production stack?
Answer: Use Change Sets:

Create a change set — CloudFormation shows exactly what will change
Review the changes (additions, modifications, replacements)
Pay special attention to Replacement: True — means resource will be deleted and recreated (downtime risk)
Execute the change set only after approval

bashaws cloudformation create-change-set --stack-name my-stack \
  --change-set-name my-review --template-body file://template.yaml

aws cloudformation describe-change-set --change-set-name my-review

aws cloudformation execute-change-set --change-set-name my-review

<img width="596" height="368" alt="image" src="https://github.com/user-attachments/assets/95f44d2b-4ee7-44ea-9760-516dcaaa6780" />
