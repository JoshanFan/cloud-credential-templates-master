# How-to create Service Accounts in AWS to provision F5XC AWS VPC or TGW site

## Prerequisites

**aws** - AWS Command Line Interface. See [here](https://aws.amazon.com/cli/) for more information.

## Steps

1. Use `aws cloudformation create-stack` command to create the stack:

    ```sh
     aws cloudformation create-stack --stack-name <STACK_NAME> \
     --template-body file://./aws-tgw-site-service-account.yaml \
     --parameters file://./parameters.json --capabilities CAPABILITY_NAMED_IAM
     ```

    * `STACK_NAME` - The name associated with the AWS CloudFormation stack. For example, f5xc-tgw-policy.
    * The template body file can be one of:
        * <https://gitlab.com/volterra.io/cloud-credential-templates/-/blob/master/aws/aws-tgw-site-service-account.yaml>
        * <https://gitlab.com/volterra.io/cloud-credential-templates/-/blob/master/aws/aws-vpc-site-service-account.yaml>
    * `Parameters` - The parameters JSON file contains the list of parameters passed to the AWS CloudFomation template.
    * `Capabilities` - Required capabilities to create the AWS CloudFormation stack.

    `NOTE` - Update the password in `parameters.json` file.

2. Use `aws cloudformation describe-stack` command to get the details of the stack created in Step 1:

    ```sh
    aws cloudformation describe-stacks --stack-name <STACK_NAME>
    ```

    * `STACK_NAME` - The stack name used in Step 1
    * The output of the above command is a JSON file which provides information about the user created by the AWS CloudFormation tempalte.
    * Note down the AccessKey and the SecretKey from the Outputs section of the resulting JSON.

The Access Key and the Secret Key can be used to create the `AWS Programmatic Access Credentials` on `F5XC Console`.

`NOTE` - The AWS CloudFormation template for F5XC AWS TGW site contains the extra permissions required on top of F5XC AWS VPC site.
