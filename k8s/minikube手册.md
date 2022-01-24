kubectl get pods 
kubectl get events 
kubectl get deployments
kubectl rollout pause deployment.v1.apps/nginx-deployment
kubectl -n <namespace-name> describe pod <pod name>
kubectl -n <namespace-name> describe pod <pod name>

### 14

kubectl attach -it <pod name> -c shell


kubectl scale --replicas=0 deployment/<your-deployment>

\# Scale a replicaset named 'foo' to 3.
kubectl scale --replicas=3 rs/foo

\# Scale a resource identified by type and name specified in "foo.yaml" to 3.
kubectl scale --replicas=3 -f foo.yaml

\# If the deployment named mysql's current size is 2, scale mysql to 3.
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql

\# Scale multiple replication controllers.
kubectl scale --replicas=5 rc/foo rc/bar rc/baz

\# Scale statefulset named 'web' to 3.
kubectl scale --replicas=3 statefulset/web



\# 筛选指定namespace 下 指定pod名字 的events 

kubectl get event --namespace <namespace-name> --field-selector involvedObject.name=<pod-name>



### 15 

$ kubectl create secret generic user --from-file=./username.txt
$ kubectl create secret generic pass --from-file=./password.txt

$ kubectl get secrets

也可以通过Secret 资源创建

# base64 转码

 echo -n 'admin' | base64

kubectl exec -it <pod-name> -- /bin/sh

kubectl create configmap <configmap name> --from-file=<file path>

kubectl get configmaps <configmap name> -o yaml

\# 不过，需要注意的是，Downward API 能够获取到的信息，一定是 Pod 里的容器进程启动之前就能够确定下来的信息。而如果你想要获取 Pod 容器运行后才会出现的信息，比如，容器进程的 PID，那就肯定不能使用 Downward API 了，而应该考虑在 Pod 里定义一个 sidecar 容器。


### 17
> 实时查看deployment 对象状态变化
kubectl rollout status deployment/<deploy-name>

> 直接编辑deployment的模板 编辑Etcd里的API对象
kubectl edit deployment/nginx-deployment

kubectl rollout undo deployment/<deploy-name>
kubectl rollout undo deployment/<deploy-name>  --to-revision=<number>

kubectl rollout history --revision=<number>

> 暂停
kubectl rollout pause 
kubectl rollout resume 


将一个集群中正在运行的多个 Pod 版本，交替地逐一升级的过程，就是“滚动更新”。
Deployment 实际上是一个两层控制器。首先，它通过 ReplicaSet 的个数来描述应用的版本；然后，它再通过 ReplicaSet 的属性（比如 replicas 的值），来保证 Pod 的副本数量。


### 18

> 一次性pod 退出就会被删除掉
kubectl run -i --tty --image busybox:1.28.4 dns-test --restart=Never --rm /bin/sh 


StatefulSet 这个控制器的主要作用之一，就是使用 Pod 模板创建 Pod 的时候，对它们进行编号，并且按照编号顺序逐一完成创建工作。而当 StatefulSet 的“控制循环”发现 Pod 的“实际状态”与“期望状态”不一致，需要新建或者删除 Pod 进行“调谐”的时候，它会严格按照这些 Pod 编号的顺序，逐一完成这些操作。

pv --> 生成pvc 
pod --> 声明使用 pvc 

所以，Kubernetes 中 PVC 和 PV 的设计，实际上类似于“接口”和“实现”的思想。开发者只要知道并会使用“接口”，即：PVC；而运维人员则负责给“接口”绑定具体的实现，即：PV。

StatefulSet --> Pods              每个pod hostname 不同 name不同 拓扑状态
StatefulSet --> Headless Service  保持访问
StatefulSet --> PVC --> PV        存储保持
只要 Statefule Service 在 ，就可以保持服务正常

StatefuleSet 保持了pod的拓扑状态和存储状态的维护

首先，StatefulSet 的控制器直接管理的是 Pod。这是因为，StatefulSet 里的不同 Pod 实例，不再像 ReplicaSet 中那样都是完全一样的，而是有了细微区别的。比如，每个 Pod 的 hostname、名字等都是不同的、携带了编号的。而 StatefulSet 区分这些实例的方式，就是通过在 Pod 的名字里加上事先约定好的编号。其次，Kubernetes 通过 Headless Service，为这些有编号的 Pod，在 DNS 服务器中生成带有同样编号的 DNS 记录。只要 StatefulSet 能够保证这些 Pod 名字里的编号不变，那么 Service 里类似于 web-0.nginx.default.svc.cluster.local 这样的 DNS 记录也就不会变，而这条记录解析出来的 Pod 的 IP 地址，则会随着后端 Pod 的删除和再创建而自动更新。这当然是 Service 机制本身的能力，不需要 StatefulSet 操心。最后，StatefulSet 还为每一个 Pod 分配并创建一个同样编号的 PVC。这样，Kubernetes 就可以通过 Persistent Volume 机制为这个 PVC 绑定上对应的 PV，从而保证了每一个 Pod 都拥有一个独立的 Volume。在这种情况下，即使 Pod 被删除，它所对应的 PVC 和 PV 依然会保留下来。所以当这个 Pod 被重新创建出来之后，Kubernetes 会为它找到同样编号的 PVC，挂载这个 PVC 对应的 Volume，从而获取到以前保存在 Volume 里的数据。

### 21

kubectl patch <操作的api名> <对象名>

DaemonSet 集群中每个节运行一个，新加入的节点自动创建一个，节点被删除pod自动回收
跟其他编排对象不一样，DaemonSet 开始运行的时机，很多时候比整个 Kubernetes 集群出现的时机都要早。

相比于 Deployment，DaemonSet 只管理 Pod 对象，然后通过 nodeAffinity 和 Toleration 这两个调度器的小功能，保证了每个节点上有且只有一个 Pod。这个控制器的实现原理简单易懂，希望你能够快速掌握。

与此同时，DaemonSet 使用 ControllerRevision，来保存和管理自己对应的“版本”。这种“面向 API 对象”的设计思路，大大简化了控制器本身的逻辑，也正是 Kubernetes 项目“声明式 API”的优势所在。