### Defining a Pod’s Readiness and Liveness Probe
```shell
k run hello --image=bonomat/nodejs-hello-world --restart=Never --port=3000 -o yaml --dry-run=client > pod.yaml

#  edit pod.yaml

k apply -f pod.yaml

k exec -it hello -- /bin/sh

k logs hello
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: hello
  name: hello
spec:
  containers:
  - image: bonomat/nodejs-hello-world
    name: hello
    ports:
    - containerPort: 3000
      name: nodejs-port
    readinessProbe:
      httpGet:
        path: /
        port: nodejs-port
      initialDelaySeconds: 2
    livenessProbe:
      httpGet:
        path: /
        port: nodejs-port
      initialDelaySeconds: 5
      periodSeconds: 8
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

### Fixing a Misconfigured Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: failing-pod
  name: failing-pod
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - while true; do echo $(date) >> ~/tmp/curr-date.txt; sleep
      5; done;
    image: busybox
    name: failing-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
```shell
k logs failing-pod
k exec failing-pod -it -- /bin/sh
# mkdir -p ~/tmp
# cd ~/tmp
# ls -l
#  cat ~/tmp/curr-date.txt
```