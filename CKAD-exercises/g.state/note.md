```shell
k run busybox --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c 'sleep 3600' > pod.yaml

k apply -f pod.yaml

k exec -it busybox  --container=busybox1 -- /bin/sh

k exec -it busybox  --container=busybox2 -- /bin/sh
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox1
    resources: {}
    volumeMounts: 
    - name: busyboxv
      mountPath: /etc/foo
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox2
    resources: {}
    volumeMounts: 
    - name: busyboxv
      mountPath: /etc/foo 
  volumes: 
  - name: busyboxv
    emptyDir: {}    
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```