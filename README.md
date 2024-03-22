https://www.youtube.com/@AntonPutra/playlists
https://www.youtube.com/watch?v=MZyrxzb7yAU

first create all resources accept 10-autoscaler.tf

to export kubernetes config (update region and cluster name accordingly)
run >> aws eks --region us-east-1 update-kubeconfig --name demo

kubectl apply -f deployment.yaml
kubectl apply -f publiclb.yaml


to add autoscaler 

1. create terraform apply - 10iam-autoscaler.tf
2. edit k8s manifest cluster-autoscaler.yaml line no10 arn with your autoscaler-iam-arn(above created by terraform)
3. edit line 150 the eks version should match to autoscaler - image: registry.k8s.io/autoscaling/cluster-autoscaler:v1.29
4. line 166 those tags should be attached to autocslaing group whichle creating autoscaling group.... 
tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/<idit ur cluster name here>  ---- - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/demo
5. kubectl apply -f .\cluster-autoscaler.yaml
6. kubectl get pods -n kube-system

to trigger autoscaler increase the no of pods in deployment.yml
