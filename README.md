# Steps to go through


Creating the first Knowledge base is a hassle, you need to put in place all the required permissions and policies. The following steps *work on my machine*.

1. Generate new AWS account. First you need to create a root account, then you need to create another account via IAM.

2. For the new account you need to give access and policies. Easiest is to create a new user group and assign it to the new user. Not all is available via the group management thing, some things you will need to add manually with inline rule. 

These you can define via group permissions:

AmazonBedrockFullAccess	
AmazonOpenSearchDirectQueryGlueCreateAccess	
AmazonOpenSearchIngestionFullAccess	
AmazonOpenSearchIngestionReadOnlyAccess	
AmazonOpenSearchServiceCognitoAccess	
AmazonOpenSearchServiceFullAccess	
AmazonOpenSearchServiceReadOnlyAccess	
AmazonS3FullAccess	
AWSQuicksightOpenSearchPolicy	
CloudWatchEventsFullAccess	
IAMFullAccess

These you need to add via inline policy: 

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "aoss:CreateSecurityPolicy",
                "aoss:CreateAccessPolicy",
                "aoss:CreateCollection",
                "aoss:ListCollections",
                "aoss:BatchGetCollection",
                "aoss:*",
                "cloudwatch:GetMetricData",
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:*",
                "bedrock:CreateKnowledgeBase",
                "bedrock:*"
            ],
            "Resource": "*"
        }
    ]
}

Not all of these you may require but I was too lazy to figure out the exact set of permissions and policies. 

3. Before creating a knowledge base you need to request access to the base models. This happens by clicking on the base models in the Amazon Bedrock management console. Note, nothing works in Ireland. Best bet is to choose something from U.S. like Oregon. You will need a base model for embeddings and output (inference). For embeddings Titan Embeddings G1 - Text works OK. In Oregon the only base model for output that worked seems to be Claude 3 Sonnet. If you don't have a base model permissions for text in place, the UI displays no options. 

4. Create S3 Bucket and upload a sample markdown file there. 

5. Create Knowledge base. Select to use the S3 Bucket and default vector database (OpenSearch - aoss). 

6. After creating knowledge base you're automatically taken to the management view of the new knowledge base. In here, click to sync the data from the S3 bucket to the KB. This should happen really fast. 

7. Select model for output (inference)

8. Test it out

9. Voila