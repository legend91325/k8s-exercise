```shell
k run twopods --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c 'e
cho hello;sleep 3600;' > pods.yaml

# vim pods.yaml
k apply -f pods.yaml
k exec twopods --container=second -it -- /bin/sh -c 'ls'

k delete pod twopods
```

```shell
k run pod --image=nginx --restart=Never --port=80 --dry-run=client -o yaml > pod-init.yaml

k get pod pod -o wide

k run test_pod  --image=busybox --rm --restart=Never -it -- /bin/sh -c 'wget -O- $IP:80'

k delete pod pod
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  initContainers:
  - image: busybox
    name: busybox
    args:
    - /bin/sh
    - -c
    - wget -O /work-dir/index.html http://neverssl.com/online
    volumeMounts:    - mountPath: /work-dir
      name: emptyvolume

  containers:
  - image: nginx
    name: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: emptyvolume
  volumes:
  - name: emptyvolume
    emptyDir: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```