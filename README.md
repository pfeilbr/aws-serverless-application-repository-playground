# aws-serverless-application-repository-playground

learn [AWS Serverless Application Repository](https://docs.aws.amazon.com/en_pv/serverlessrepo)

## Example using `aws serverlessrepo` CLI

> can also be done (and is preferred) via sam cli

```sh

APP_NAME=myapp01

aws serverlessrepo list-applications

# create appliction
aws serverlessrepo create-application \
--author 'Brian Pfeil' \
--description $APP_NAME \
--name $APP_NAME \
--semantic-version 0.0.1 \
--template-body file://./s3-bucket-template.yaml

# fetch ApplicationId
APP_ID=$(aws serverlessrepo list-applications | jq --raw-output ".Applications[] | select(.Name == \"$APP_NAME\").ApplicationId")

# create request to create cfn template (async operation)
TEMPLATE_ID=$(aws serverlessrepo create-cloud-formation-template --application-id "$APP_ID" | jq --raw-output ".TemplateId")

# fetch the cfn template
TEMPLATE_URL=$(aws serverlessrepo get-cloud-formation-template --application-id "$APP_ID" --template-id "$TEMPLATE_ID" | jq --raw-output ".TemplateUrl")

# view cfn template
curl "$TEMPLATE_URL"

# delete app
aws serverlessrepo delete-application --application-id "$APP_ID"

```