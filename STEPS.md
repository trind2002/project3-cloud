### 1. Create an S3 bucket
1. Update Permissions tab
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
2. Add the CORS configuration(opens in a new tab)

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
### 2. Set config Circle CI & update variable in the files of the folder deloyment
POSTGRES_HOST
AWS_BUCKET
...
### 3. Create EKS
eksctl create cluster --name myCluster1206 --region=us-east-1 --nodes-min=2 --nodes-max=3

eksctl delete cluster --region=us-east-1 --name=myCluster1106
kubectl get nodes // check nodes to ready
aws eks update-kubeconfig --region us-east-1 --name myCluster1206
### 4. Apply env variables and secrets & Deployments
kubectl apply -f aws-secret.yaml
kubectl apply -f env-secret.yaml
kubectl apply -f env-configmap.yaml

kubectl apply -f backend-feed-deployment.yaml
kubectl apply -f backend-feed-service.yaml

kubectl apply -f backend-user-deployment.yaml
kubectl apply -f backend-user-service.yaml

kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml

kubectl apply -f reverseproxy-deployment.yaml
kubectl apply -f reverseproxy-service.yaml

kubectl apply -f pod-test.yaml

## Check the deployment names and their pod status
kubectl get deployments

kubectl delete -f backend-feed-deployment.yaml && kubectl apply -f backend-feed-deployment.yaml
kubectl delete -f backend-feed-service.yaml && kubectl apply -f backend-feed-service.yaml

kubectl create configmap my-config --from-file=/etc/configs


kubectl delete -f backend-feed.yaml && kubectl apply -f backend-feed.yaml

### 5. The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.
kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend
kubectl expose deployment reverseproxy --type=LoadBalancer --name=publicreverseproxy

