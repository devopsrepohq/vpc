Use this CDK stack to create a standard VPC.

![VPC architecture](https://images.prismic.io/devopsrepo/e115c79a-8a7d-460b-b3a1-58d1df2edffc_vpc.png?auto=compress,format)

## What is it?

Provision a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define.

## Features

- [x] Deploy a standard VPC with public, private, and isolated subnet.
- [x] Use multiple AZs deployments for high availability.
- [x] Use flow logs to capture information about IP traffic going to and from network interfaces in VPC.

## Prerequisites

You will need the following before utilize this CDK stack:

- [AWS CLI](https://cdkworkshop.com/15-prerequisites/100-awscli.html)
- [AWS Account and User](https://cdkworkshop.com/15-prerequisites/200-account.html)
- [Node.js](https://cdkworkshop.com/15-prerequisites/300-nodejs.html)
- [IDE for your programming language](https://cdkworkshop.com/15-prerequisites/400-ide.html)
- [AWS CDK Tookit](https://cdkworkshop.com/15-prerequisites/500-toolkit.html)
- [AWS Toolkit VSCode Extension](https://github.com/devopsrepohq/aws-toolkit)

## Stack Explain

### cdk.json

Define project-name and env context variables in cdk.json

```json
{
  "context": {
    "project-name": "container",
    "env": "dev",
    "profile": "devopsrepo"
  }
}
```

### lib/vpc-stack.ts

Setup standard VPC with public, private, and isolated subnets.

```javascript
const vpc = new ec2.Vpc(this, 'Vpc', {
  maxAzs: 3,
  natGateways: 1,
  cidr: '10.0.0.0/16',
  subnetConfiguration: [
    {
      cidrMask: 24,
      name: 'ingress',
      subnetType: ec2.SubnetType.PUBLIC,
    },
    {
      cidrMask: 24,
      name: 'application',
      subnetType: ec2.SubnetType.PRIVATE,
    },
    {
      cidrMask: 28,
      name: 'rds',
      subnetType: ec2.SubnetType.ISOLATED,
    }
  ]
});
```

- maxAzs - Define 3 AZs to use in this region.
- natGateways - Create only 1 NAT Gateways/Instances.
- cidr - Use '10.0.0.0/16' CIDR range for the VPC.
- subnetConfiguration - Build the public, private, and isolated subnet for each AZ.

Create flowlog and log the vpc traffic into cloudwatch

```javascript
vpc.addFlowLog('FlowLog');
```

Deploy the stack to your aws account.

```bash
cdk deploy
or
cdk deploy --profile your_profile_name
```

## Useful commands

### NPM commands

 * `npm run build`   compile typescript to js
 * `npm run watch`   watch for changes and compile
 * `npm run test`    perform the jest unit tests

### Toolkit commands

 * `cdk list (ls)`            Lists the stacks in the app
 * `cdk synthesize (synth)`   Synthesizes and prints the CloudFormation template for the specified stack(s)
 * `cdk bootstrap`            Deploys the CDK Toolkit stack, required to deploy stacks containing assets
 * `cdk deploy`               Deploys the specified stack(s)
 * `cdk deploy '*'`           Deploys all stacks at once
 * `cdk destroy`              Destroys the specified stack(s)
 * `cdk destroy '*'`          Destroys all stacks at once
 * `cdk diff`                 Compares the specified stack with the deployed stack or a local CloudFormation template
 * `cdk metadata`             Displays metadata about the specified stack
 * `cdk init`                 Creates a new CDK project in the current directory from a specified template
 * `cdk context`              Manages cached context values
 * `cdk docs (doc)`           Opens the CDK API reference in your browser
 * `cdk doctor`               Checks your CDK project for potential problems

## Pricing

As this cdk stack will create NAT Gateway, please refer the following link for pricing

- [NAT Gateway Pricing](https://aws.amazon.com/vpc/pricing/#natgatewaypricing)