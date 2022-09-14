# CKA-Exam-Preparation-Notes

# Q1

Context using kubectl and no kubectl commands
```
kubectl config get-contexts

kubectl config get-contexts --no-headers | awk {'print $2'} > /opt/course/1/contexts
OR
k config get-contexts -o name > /opt/course/1/contexts

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
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
Fix Error:
```
k edit deployments.apps -n kube-system 
```
![image](https://user-images.githubusercontent.com/54164634/190094549-015cc38a-87cb-4a98-ab0d-3c8920c1d8ef.png)

```
k get deployments.apps -n kube-system 
kubectl top nodes
```

```
echo "kubectl top nodes" > /opt/course/7/node.sh
bash /opt/course/7/node.sh
```
```
echo "kubectl top pods" > /opt/course/7/pod.sh
bash /opt/course/7/pod.sh
```

# Q8

Identify PODs, Static PODs, Processes etc.

```
ssh cluster1-master1
k get pod -n kube-system
k get deploy -n kube-system 
k get all -n kube-system 
```
```
ps -ef | grep -i kubelet
cd /etc/kubernetes/manifests/
```


//output of /opt/course/8/master-components.txt

@**kubelet**: [process]
@**kube-apiserver**: [static-pod]
@**kube-scheduler**: [static-pod]
@**kube-controller-manager**: [static-pod]
@**etcd**: [static-pod]
@**dns**: [pod][coredns]


# Q9

Test POD scheduling using kube-scheduler
```
kubectl config use-context k8s-c2-AC 
```
```
ssh cluster2-master1
```
```
k get pod -n kube-system 
cd /etc/kubernetes/manifests/
mv kube-scheduler.yaml /etc/kubernetes/
```

ADD: nodeName: cluster2-master1
```
k run manual-schedule --image=httpd:2.4-alpine
k edit pod manual-schedule
k replace --force -f /tmp/kubectl-edit-2160969323.yaml
```

```
mv /etc/kubernetes/kube-scheduler.yaml /etc/kubernetes/manifests/  
k run manual-schedule2 --image=httpd:2.4-alpine
```

# Q10

SA, Role, Rolebinding

```
k create sa processor -n project-hamster
k create role processor --resource=secrets,configmaps --verb=create -n project-hamster
k create rolebinding processor --role processor --serviceaccount=project-hamster:process -n project-hamster 
```

```
k get sa,role,rolebinding -n project-hamster
```

 
# Q11

```
kubectl config use-context k8s-c1-H
```


 
 
 
# Q12
 
 
 
 
 
 
# Q13
 
 
 
 
 
# Q14
 
 
 
 
 
# Q15
 
 
 
 
 
# Q16




# Q17





# Q18





# Q19




# Q20




# Q21





# Q22




# Q23





# Q24



