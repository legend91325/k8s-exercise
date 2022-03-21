```shell
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml

k describe pod nginx
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
    livenessProbe:
      exec:
        command:
        - ls
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```


```shell
k run nginx --image=nginx --dry-run=client -o yaml --restart=Never --port=80 > pod.yaml

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    ports:
      - containerPort: 80 # Note: Readiness probes runs on the container during its whole lifecycle. Since nginx exposes 80, containerPort: 80 is not required for readiness to work.
    readinessProbe: # declare the readiness probe
      httpGet: # add this line
        path: / #
        port: 80 #
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```shell
k get ns # check namespaces
k -n qa get events | grep -i "Liveness probe failed"
k -n alan get events | grep -i "Liveness probe failed"
k -n test get events | grep -i "Liveness probe failed"
k -n production get events | grep -i "Liveness probe failed"
```


```shell
k run busybox --image=busybox --restart=Never -- /bin/sh -c 'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done'
k logs busybox -f 

```

```shell
k top node
```