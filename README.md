# CKA-Exam-Preparation-Notes

# Q1

Context using kubectl and no kubectl commands
```
kubectl config get-contexts

kubectl config get-contexts --no-headers | awk {'print $2'} > /opt/course/1/contexts

echo "kubectl config current-context" > /opt/course/1/context_default_kubectl.sh
bash /opt/course/1/context_default_kubectl.sh

echo "cat .kube/config | grep -i current-context" > /opt/course/1/context_default_no_kubectl.sh
bash /opt/course/1/context_default_no_kubectl.sh 
 
```

# Q2

Pod-Node Affinity
```
alias k=kubectl

kubectl config use-context k8s-c1-H

k get nodes --show-labels

k run pod1 --image=httpd:2.4.41-alpine --dry-run=client -o yaml >> q2.yaml
vim q2.yaml
//add nodeName
```

# Q3

Scaling Replicas
```
k -n project-c13 scale statefulset 03db --replicas=1
```
# Q4

Liveness Probe/Readiness Probe
```
kubectl config use-context k8s-c1-H

k run ready-if-service-ready --image=nginx:1.16.1-alpine --dry-run=client -o yaml > q4pod1.yaml
k apply -f q4pod1.yaml 

k run am-i-ready --image=nginx:1.16.1-alpine --labels=id=cross-server-ready
k describe svc service-am-i-ready
```

# Q5

Use full command (kubectl) for shell scripts 

```
echo "kubectl get pod -A --sort-by=.metadata.creationTimestamp" > /opt/course/5/find_pods.sh
bash find_pods.sh

echo "kubectl get pod -A --sort-by=.metadata.uid" > find_pods_uid.sh
bash find_pods_uid.sh 
```

# Q6
Create PV , PVC , Deployment 

```
k create -f q6pv.yaml 
k create -f q6pvc.yaml 
k get pv,pvc -n project-tiger 

k create deployment safari --image=httpd:2.4.41-alpine -n project-tiger --dry-run=client -o yaml > q6dep.yaml
k get pv,pvc,deployments.apps -n project-tiger 
```

# Q7

kubectl config use-context k8s-c1-H

Install Metrics Server :
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

Fix Error:
k edit deployments.apps -n kube-system 

![image](https://user-images.githubusercontent.com/54164634/190094549-015cc38a-87cb-4a98-ab0d-3c8920c1d8ef.png)


k get deployments.apps -n kube-system 
kubectl top nodes


k get deployments.apps -n kube-system 


