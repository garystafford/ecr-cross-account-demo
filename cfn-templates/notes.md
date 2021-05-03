# CloudFormation: CodeBuild Master Template

```text
# change me
export ISV_ACCOUNT=111222333444
export ISV_ECR_REGION=us-east-2
export CUSTOMER_ACCOUNT=999888777666
export CUSTOMER_ECR_REGION=us-west-2

aws cloudformation create-stack \
    --stack-name codebuild-rake-app \
    --template-body file://cfn-templates/codebuild_repo.yml \
    --parameters \
        ParameterKey=ProjectName,ParameterValue=rake-app \
        ParameterKey=EcrAccountId,ParameterValue=$ISV_ACCOUNT \
        ParameterKey=EcrRegion,ParameterValue=$ISV_ECR_REGION \
        ParameterKey=ImageTag,ParameterValue=latest \
        ParameterKey=ServiceRoleArn,ParameterValue=codebuild-nlp-client-service-role \
    --capabilities CAPABILITY_NAMED_IAM

aws codebuild start-build --project-name rake-app 

aws cloudformation create-stack \
    --stack-name codebuild-nlp-client \
    --template-body file://cfn-templates/codebuild_repo.yml \
    --parameters \
        ParameterKey=ProjectName,ParameterValue=nlp-client \
        ParameterKey=EcrAccountId,ParameterValue=$CUSTOMER_ACCOUNT \
        ParameterKey=EcrRegion,ParameterValue=$CUSTOMER_ECR_REGION \
        ParameterKey=ImageTag,ParameterValue=latest \
        ParameterKey=ServiceRoleArn,ParameterValue=codebuild-nlp-client-service-role \
    --capabilities CAPABILITY_NAMED_IAM

aws codebuild start-build --project-name rake-app 
```