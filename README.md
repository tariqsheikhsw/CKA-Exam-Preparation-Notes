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


