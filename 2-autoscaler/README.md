eksctl create nodegroup --config-file eks-autoscaler.yml

kubectl get nodes

If you want delete a nodegroup:
eksctl delete nodegroup -f eks-autoscaler.yml --include="ng-1" --approve

Deploy autoscaler to cluster:

kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml

To ensure that our autoscaler deployment do not be removed
kubectl -n kube-system annotate deployment.apps/cluster-autoscaler cluster-autoscaler.kubernetes.io/safe-to-evict="false" 

Get version:
https://github.com/kubernetes/autoscaler/releases

Edit deployment:
kubectl -n kube-system edit deployment cluster-autoscaler
  Replace: YOUR_CLUSTER_NAME
  Edit: image version of autoscaler
  --balance-similar-node-groups
  --skip-nodes-with-system-pods=false


https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html#ca-ng-considerations

Edit the image:
kubectl -n kube-system set image deployment.apps/cluster-autoscaler cluster-autoscaler=us.gcr.io/k8s-artifacts-prod/autoscaling/cluster-autoscaler:v1.16.5


kubectl -n kube-system get deployment
kubectl logs -f deployment/cluster-autoscaler -n kube-system
kubectl -n kube-system describe deployments cluster-autoscaler

Testing autoscaler:
kubectl apply -f nginx-deployment.yml
kubectl get pods

Show number of node filtering:
kubectl get nodes -l instanceType=onDemand 
or 
kubectl get nodes -l nodegroup-type=scale-workload 

kubectl scale --replicas=3 deployment/test-autoscaler


If update yml file adding logging:
eksctl utils update-cluster-logging --config-file eks-autoscaler.yml --approve
eksctl utils update-cluster-logging --name=EKS-hibicode-cluster --disable-types all --approve


Application Load Balancer

https://kubernetes-sigs.github.io/aws-alb-ingress-controller/guide/walkthrough/echoserver/

wget https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.6/docs/examples/alb-ingress-controller.yaml
wget https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.6/docs/examples/rbac-role.yaml

