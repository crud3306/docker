

所用集群的配置
------------
xxx.xxx.xxx.xxx	kubectl-5-4.conf	bootcamp_7




三个最重要和最常用的逻辑单元
------------
Pods：业务逻辑单位

Deployment：记录版本更新信息，通过控制replicaset，完成版本更新，回滚等操作

ReplicaSet : 控制pods数量

Service ： 使得pods能够被外部或k8s集群内访问到





kubectl基本命令
-------------
```
Get help with kubectl commands:
kubectl help

Get the state of your cluster:
kubectl cluster-info

Get all the nodes of your cluster:
kubectl get nodes -o wide

Get information about the pods of your cluster:
kubectl get pods
kubectl get pods -o wide

Get information about the replication controllers of your cluster:
kubectl get rc -o wide

Get information about the deployments of your cluster:
kubectl get deployments

Get information about the services of your cluster:
kubectl get services

Get full configuration information about a service:
kubectl get service <instancename> -o json

Get the IP address of a pod:
kubectl get pod <instancename> -template={{.status.podIP}}

Delete a pod:
kubectl delete pod <pod instancename>

Delete a deployment:
kubectl delete deployment <deployment instancename>

Delete a service:
kubectl delete service <service instancename>


show detailed information about a resource
kubectl describe <>

print the logs from a container in a pod
kubectl logs <>

execute a command on a container in a pod
kubectl exec <>

```



kubectl示例
-------------
1. CCE上创建集群
```
在cloud.baidu.com上选择产品CCE
选择区域，然后创建集群
获得集群IP
并下载集群配置文件保存到本地（例如Mac），save as ～/.kube/config
下载kubectl
运行kubectl get nodes
```

2. CCE上创建deployement和pod
```
$kubectl get deployments
$kubectl run kubernetes-bootcamp --image=hub.baidubce.com/bootcamp/mynode:1.0.0 --port=8080 
$kubectl get deployment
$kubectl get pods
$kubectl proxy &
$export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
$curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
$kubectl logs $POD_NAME
$kubectl exec $POD_NAME env
$kubectl exec -ti $POD_NAME bash
#cat server.js
#exit	
```

3. 在CCE上创建Service
```
$kubectl get services
$kubectl scale --replicas=3 deployment/kubernetes-bootcamp
$kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
$kubectl get services
$kubectl describe services/kubernetes-bootcamp
$export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}’)
$curl $VM_IP:$NODE_PORT
$while(true); do curl http://$VM_IP:$NODE_PORT; sleep 1; done
```

4. 使用标签
```
$kubectl get pods
$kubectl get pods -l run=kubernetes-bootcamp
$kubectl get services -l run=kubernetes-bootcamp
$kubectl label pod $POD_NAME a=c
$kubectl get pods -l a=c
$kubectl describe pod $POD_NAME
```







