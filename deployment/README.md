## Apply env variables and secrets
kubectl apply -f aws-secret.yaml
kubectl apply -f env-secret.yaml
kubectl apply -f env-configmap.yaml
## Deployments - Double check the Dockerhub image name and version in the deployment files
kubectl apply -f backend-feed-deployment.yaml
## Do the same for other three deployment files
## Service
kubectl apply -f backend-feed-service.yaml
## Do the same for other three service files


Expose External IP
## Check the deployment names and their pod status
kubectl get deployments
## Create a Service object that exposes the frontend deployment
## The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.
kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend
## Repeat the process for the *reverseproxy* deployment. 
## Check name, ClusterIP, and External IP of all deployments
kubectl get services 
kubectl get pods # It should show the STATUS as Running



Update the Environment Variables and Re-Deploy the Application
Update udagram-frontend/src/environments/environment.prod.ts file in the same way as done for the environment.ts file.
## Run these commands from the /udagram-frontend directory
docker build . -t [Dockerhub-username]/udagram-frontend:v6
docker push [Dockerhub-username]/udagram-frontend:v6
## Run these commands from the /udagram-deployment directory
## Rolling update the containers of "frontend" deployment
kubectl set image deployment frontend frontend=[Dockerhub-username]/udagram-frontend:v6