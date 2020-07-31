# VPC

Use this CDK stack to create a standard VPC.

![VPC architecture](https://github.com/devopsrepohq/vpc/blob/master/_docs/vpc.png?raw=true)

# Features

- [x] Deploy a standard VPC with public, private, and isolated subnet.
- [x] Use multiple AZs deployments for high availability.
- [x] Use flow logs to capture information about IP traffic going to and from network interfaces in VPC.

# Prerequisites

You will need the following before utilize this CDK stack:

- [AWS CLI](https://cdkworkshop.com/15-prerequisites/100-awscli.html)
- [AWS Account and User](https://cdkworkshop.com/15-prerequisites/200-account.html)
- [Node.js](https://cdkworkshop.com/15-prerequisites/300-nodejs.html)
- [IDE for your programming language](https://cdkworkshop.com/15-prerequisites/400-ide.html)
- [AWS CDK Tookit](https://cdkworkshop.com/15-prerequisites/500-toolkit.html)

# Stack Explain

## lib/vpc-stack.ts

Setup standard VPC with public, private, and isolated subnets.

```
const vpc = new ec2.Vpc(this, 'MyVpc', {
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

Create flowlog and log the vpc's reject traffic into cloudwatch

```
vpc.addFlowLog('FlowLogCloudWatch', {
  trafficType: ec2.FlowLogTrafficType.REJECT     
});
```

# Useful commands

## NPM commands

 * `npm run build`   compile typescript to js
 * `npm run watch`   watch for changes and compile
 * `npm run test`    perform the jest unit tests

## Toolkit commands

 * `cdk list (ls)`            Lists the stacks in the app
 * `cdk synthesize (synth)`   Synthesizes and prints the CloudFormation template for the specified stack(s)
 * `cdk bootstrap`            Deploys the CDK Toolkit stack, required to deploy stacks containing assets
 * `cdk deploy`               Deploys the specified stack(s)
 * `cdk deploy '*'`           Deploys all stacks at once
 * `cdk destroy`              Destroys the specified stack(s)
 * `cdk diff`                 Compares the specified stack with the deployed stack or a local CloudFormation template
 * `cdk metadata`             Displays metadata about the specified stack
 * `cdk init`                 Creates a new CDK project in the current directory from a specified template
 * `cdk context`              Manages cached context values
 * `cdk docs (doc)`           Opens the CDK API reference in your browser
 * `cdk doctor`               Checks your CDK project for potential problems

 # Pricing

As this cdk stack will create NAT Gateway, please refer the following link for pricing

- [NAT Gateway Pricing](https://aws.amazon.com/vpc/pricing/#natgatewaypricing)
