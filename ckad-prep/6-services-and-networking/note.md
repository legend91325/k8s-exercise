### Routing Traffic to Pods from Inside and Outside of a Cluster
```shell
k create deployment myapp --image=nginx --replicas=2 --port=80

k get deployments.apps

k expose deployment  myapp --target-port=80 --port=80

k get service
#NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
# myapp        ClusterIP   10.40.11.42   <none>        80/TCP    4s

k run tmp-pod --image=busybox --restart=Never -it --rm -- wget -O- 10.40.11.42:80

kubectl edit service myapp
# type: NodePort

kubectl get services
# NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE    SELECTOR
# kubernetes   ClusterIP   10.40.0.1     <none>        443/TCP        4d4h   <none>
# myapp        NodePort    10.40.11.42   <none>        80:31873/TCP   7m     app=myapp
```

### Restricting Access to and from a Pod
```shell
 k create namespace  app-stack

 k apply -f app-stack.yaml

 k get pods --namespace app-stack 

 k get networkpolicies.networking.k8s.io  --namespace app-stack
```
```yaml
kind: Pod
apiVersion: v1
metadata:
  name: frontend
  namespace: app-stack
  labels:
    app: todo
    tier: frontend
spec:
  containers:
    - name: frontend
      image: nginx

---

kind: Pod
apiVersion: v1
metadata:
  name: backend
  namespace: app-stack
  labels:
    app: todo
    tier: backend
spec:
  containers:
    - name: backend
      image: nginx

---

kind: Pod
apiVersion: v1
metadata:
  name: database
  namespace: app-stack
  labels:
    app: todo
    tier: database
spec:
  containers:
    - name: database
      image: mysql
      env:
      - name: MYSQL_ROOT_PASSWORD
        value: example
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: app-stack-network-policy
  namespace: app-stack
spec:
  podSelector:
    matchLabels:
      app: todo
      tier: database
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: todo
          tier: backend
    ports:
    - protocol: TCP
      port: 3306
```