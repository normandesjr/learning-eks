````
eksctl create cluster -f eks-basic.yml --kubeconfig=./eks-hibicode-cluster.yaml
````

kubectl get nodes

eksctl get cluster

eksctl get nodegroup --cluster EKS-hibicode-cluster

eksctl scale nodegroup --cluster EKS-hibicode-cluster --nodes 5 --nodes-max 5 --name ng-1
eksctl scale nodegroup --cluster EKS-hibicode-cluster --nodes 3 --nodes-max 5 --name ng-1


