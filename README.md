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

Create DaemonSet using template:
https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

```
kubectl config use-context k8s-c1-H
```

```
k create -f q11.yaml 
k get ds -n project-tiger
```


# Q12
Deployment acting as DaemonSet (One Pod per Node)

```
k create deployment deploy-important --image=nginx:1.17.6-alpine -n project-tiger --replicas=3 --dry-run=client -o yaml > q12.yaml
k get pods -n project-tiger 
k replace --force -f q12.yaml
```
 
 
# Q13
 Volume should NOT be shared per POD
 
 ```
 k run multi-container-playground --image=nginx:1.17.6-alpine --dry-run=client -o yaml > q13.yaml
 k replace --force -f q13.yaml 
 k get pod
 k describe pod multi-container-playground 
 ```
 //path can be adjusted 
 //can use args also in addition to command
 
```
  - image: busybox
    name: c2
    command: ["bin/sh", "-c"]
    args:
    - while true; do
        date >> /vol/date.log;
        sleep 1;
      done
    volumeMounts:
    - mountPath: /vol
      name: volume
  - image: busybox
    name: c3
    command: ["bin/sh","-c"]
    args:
    - tail -f /vol/date.log
 ```
Logs
```
k logs multi-container-playground c1
k logs multi-container-playground c2
k logs multi-container-playground c3
```

# Q14
 
```
/opt/course/14/cluster-info
q1:How many master nodes are available?
1
[k get node]

q2:How many worker nodes are available?
2
[k get node]

q3:What is the Pod CIDR of cluster1-worker1?
10.244.1.0/24
[k describe node | less -p PodCIDR]

q4:What is the Service CIDR?
10.96.0.0/12
[cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep range]
OR
[k get pod kube-apiserver-controlplane -n kube-system -o yaml | grep -i service]

q5:Which Networking (or CNI Plugin) is configured and where is its config file?
Weave, /etc/cni/net.d/10-weave.conflist
OR (in some cases)
Calico, /etc/cni/net.d/calico-kubeconfig, /etc/cni/net.d/10-canal.conflist
[find /etc/cni/net.d/]

q6:Which suffix will static pods have that run on cluster1-worker1?
-cluster1-worker1
[k get pod]
```
 
# Q15

```
kubectl get events -A --sort-by=.metadata.creationTimestamp
echo "kubectl get events -A --sort-by=.metadata.creationTimestamp" > /opt/course/15/cluster_events.sh
bash /opt/course/15/cluster_events.sh

k get events -n kube-system > /opt/course/15/pod_kill.log

crictl ps | grep kube-proxy
crictl stop ab4ae2d9784d7

k get events -n kube-system > /opt/course/15/container_kill.log
```

 
 
 
# Q16
Namespaced Resources

```
k create ns cka-master
k api-resources --namespaced -o name > /opt/course/16/resources.txt
OR
k api-resources --namespace=true | awk {'print $1'} > /opt/course/16/resources.txt
```

# Q17





# Q18





# Q19




# Q20




# Q21





# Q22




# Q23





# Q24



