Fix issue:
1. when run cmd: ionic build
If error: error:0308010C:digital envelope routines::unsupported
Run this command to fix the issue:
$export NODE_OPTIONS=--openssl-legacy-provider

2. Create an S3 bucket
a. go to the Permissions tab
{
 "Version":"2012-10-17",
 "Statement":[
     {
         "Sid":"Stmt1625306057759",
         "Principal":"*",
         "Action":"s3:*",
         "Effect":"Allow",
         "Resource":"arn:aws:s3:::[bucket-name]"
     }
 ]
}
b. Add the CORS configuration(opens in a new tab)
[
	{
		"AllowedHeaders":[
			"*"
		],
		"AllowedMethods":[
			"POST",
			"GET",
			"PUT",
			"DELETE",
			"HEAD"
		],
		"AllowedOrigins":[
			"*"
		],
		"ExposeHeaders":[
			
		]
	}
]

3.  Create aws-secret.yaml file to store your AWS login credentials
## Use a combination of head/tail command to identify lines you want to convert to base64
## You just need two correct lines: a right pair of aws_access_key_id and aws_secret_access_key
cat ~/.aws/credentials | tail -n 5 | head -n 2
## Convert 
cat ~/.aws/credentials | tail -n 5 | head -n 2 | base64


Steps to deloyment k8s

1. We can create eks-cluster with the commands:
https://eksctl.io/usage/creating-and-managing-clusters/
eksctl create cluster -f ./deployment/ekst.yaml
eksctl delete cluster -f cluster.yaml
2. update config
aws eks update-kubeconfig --region region-code --name my-cluster