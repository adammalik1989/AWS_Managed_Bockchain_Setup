# AWS_Managed_Bockchain_Setup


This a detailed description of setting up the AWS Managed Blockchain Service and  Blockchain Client Node.
There are two components here we will work on:
* AWS Managed Blockchain Service 
* Blockchain Client Node.

PICTURE of aAWS architecture





## Setting up the Client Node  ( for e.g. BMW AG)


I have used the CLoud9 IDE by AWS because In the beginning I was working in the private VPC and there was no easy way to connect to tthe 
to the private VPC.

Step 1: (Cloud9 instance setup, not mandatory.I will just use its terminal.)

1. Start the AWS Cloud9 instance.
2. Clone the repo :

```
cd ~
git clone https://github.com/aws-samples/non-profit-blockchain.git

```

Update the AWS CLI 

```
sudo pip install awscli --upgrade
```



## Create Fabric Client Node 

The fabric client node will host the Fabric CLI.The  fabric client will be created in its own VPC in your AWS account.
I have attached the Cloud Formation Template which will create the Fabric Clien node, the vpc and endpoints.

The cloud formation template requires a number of parameter values.We have export these variables before running the script.

In Cloud9:

```
export REGION=us-east-1
export NETWORKID=<the network ID you created in Step1, from the Amazon Managed Blockchain Console>
export NETWORKNAME=<the name you gave the network>
```

Now check the VPC endpoint and print it out.

```
export VPCENDPOINTSERVICENAME=$(aws managedblockchain get-network --region $REGION --network-id $NETWORKID --query 'Network.VpcEndpointServiceName' --output text)
echo $VPCENDPOINTSERVICENAME
```

















