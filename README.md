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
export REGION=<us-east-1> 
export NETWORKID=<the network ID you created , from the Amazon Managed Blockchain Console>
export NETWORKNAME=<the name you gave the network>
```

Now check the VPC endpoint and print it out.

```
export VPCENDPOINTSERVICENAME=$(aws managedblockchain get-network --region $REGION --network-id $NETWORKID --query 'Network.VpcEndpointServiceName' --output text)
echo $VPCENDPOINTSERVICENAME
```

If the VPC endpoint is there with a value then run the given script.It will create  Cloud Formation Stack.
You will see an error saying `key pair does not exist`. This is expected as the script
will check whether the keypair exists before creating it.
Developers from AWS has written the script in such a fashion that It won't overwrite any existing keypairs.


```
cd ~/non-profit-blockchain/ngo-fabric
./3-vpc-client-node.sh
```



## Step 3  Enroll the identity in the Fabric client Node


SSH into your client node created from the Cloud Formation Stack.

```
cd ~
ssh ec2-user@<dns of EC2 instance> -i ~/< name>-keypair.pem
```

Before enrolling the identity ,we need to do the following things:
* Export the ENV variables that provides a context to Hyperledger Fabric.


Create the file that includes the ENV export values that define your Fabric network configuration.

```
cd ~/non-profit-blockchain/ngo-fabric
cp templates/exports-template.sh fabric-exports.sh
vi fabric-exports.sh
```

Now update the export statements and then source the file again.

````cd ~/non-profit-blockchain/ngo-fabric
source fabric-exports.sh

````

Sourcing the file will do two things:

* export the necessary ENV variables
* create another file which contains the export values you need to use when working with a Fabric peer node. This can be    found in the file: `~/peer-exports.sh.` You will see how to use this in a later step.


Check the source worked by typing the given command:

```
$ echo $PEERSERVICEENDPOINT
nd-4MHB4EKFCRF7VBHXZE2ZU4F6GY.m-B7YYBFY4GREBZLPCO2SUS4GP3I.n-WDG36TTUD5HEJORZUPF4REKMBI.managedblockchain.us-east-1.amazonaws.com:30003

```
Now we need the latest version of PEM file.  It will overwrite the exisiting file in the home directory.

```
aws s3 cp s3://us-east-1.managedblockchain/etc/managedblockchain-tls-chain.pem  /home/ec2-user/managedblockchain-tls-chain.pem
```



























