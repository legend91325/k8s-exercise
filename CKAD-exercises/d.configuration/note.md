```shell
k create configmap --from-literal=foo=lala --from-literal=foo2=lolo

k create configmap config --from-literal=foo=lala --from-literal=foo2=lolo

k get configmaps config

k describe configmaps config

echo -e 'foo3=lili\nfoo4=lele' > config.txt

k create configmap config2 --from-file=config.txt

k create configmap config4 --from-file=special=config.txt

k get configmaps config4 -o yaml

k run  nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml 

k exec -it nginx -- env |grep option

k  create configmap anotherone --from-literal=var6=val6 --from-literal=var7=val7


```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2022-01-24T07:13:33Z"
  labels:
    run: nginx
  name: nginx
  namespace: default
  resourceVersion: "6257976"
  uid: 83bde141-45e8-42e9-8a08-50ddefdf7bfa
spec:
  containers:
  - image: nginx
    imagePullPolicy: Always
    name: nginx
    resources: {}
    envFrom:
    - configMapRef:
        name: anotherone
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-f5vdq
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Never
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-f5vdq
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  phase: Pending
  qosClass: BestEffort
```


```shell
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml

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
  securityContext:
    runAsUser: 101
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
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
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
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
    resources:
      limits:
        memory: "512Mi"
        cpu: "200"
      requests:
        memory: "256Mi"
        cpu: "100"
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```shell
k create  secret generic mysecret --from-literal=password=mypass

echo -n admin > username

k create secret generic mysecret2 --from-file=username

k get secrets mysecret2 -o yaml
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
  volumes: # specify the volumes
  - name: foo # this name will be used for reference inside the container
    secret: # we want a secret
      secretName: mysecret2 # name of the secret - this must already exist on pod creation
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    volumeMounts: # our volume mounts
    - name: foo # name on pod.spec.volumes
      mountPath: /etc/foo #our mount path
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
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
    env: # our env variables
    - name: USERNAME # asked name
      valueFrom:
        secretKeyRef: # secret reference
          name: mysecret2 # our secret's name
          key: username # the key of the data in the secret
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```