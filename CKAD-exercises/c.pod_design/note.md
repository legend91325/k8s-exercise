```shell
k run nginx1 --image=nginx --restart=Never --labels=app=v1
k run nginx2 --image=nginx --restart=Never --labels=app=v1
k run nginx3 --image=nginx --restart=Never --labels=app=v1

k get pods --show-labels

k label pod nginx2 app=v2

k get pods -l app --show-labels

k get pods -l app=v2 --show-labels

k label pods -l app app-
```


```shell
k get nodes --show-labels

k label nodes gke-gke-default-pool-b59a111e-qnqq accelerator=nvidia-tesla-p100

k run label-node --image=busybox --restart=Never --dry-run=client -o yaml> pod.yaml

k describe pod label-node
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: label-node
  name: label-node
spec:
  containers:
  - image: busybox
    name: label-node
    resources: {}
  nodeSelector:
    accelerator: nvidia-tesla-p100
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```shell
k annotate pod nginx1 nginx2 nginx3 description='my description'

k annotate pod nginx1 --list

k annotate pod nginx1 nginx2 nginx3 description-

k delete pod nginx1 nginx2 nginx3
```

```shell
k create deployment nginx --image=nginx:1.18.0 --replicas=2 --port=80

k get deployments.apps

k get deployments.apps nginx -o yaml

k get replicasets.apps nginx-67dfd6c8f9 -o yaml

k get pod nginx-67dfd6c8f9-2b6j8 -o yaml

k rollout status deployment nginx

k set image deployment nginx nginx=nginx:1.19.8

k rollout history deployment nginx

k rollout undo deployment nginx --to-revision=1

k get deployments.apps nginx -o yaml

k set image deployment nginx nginx=nginx:1.91

k rollout status deployment nginx

k rollout undo deployment nginx --to-revision=3

k rollout history deployment nginx --revision=4

k scale deployment nginx --replicas=5

k autoscale deployment nginx --min=5 --max=10 --cpu-percent=80

k get horizontalpodautoscalers.autoscaling nginx

k rollout pause deployment nginx

k set image deployment nginx nginx=nginx:1.19.9

k rollout history deployment nginx

k rollout status deployment nginx

k rollout resume deployment nginx

k rollout history deployment nginx

k delete deployments.apps nginx

k delete horizontalpodautoscalers.autoscaling nginx
```

```shell
k create job pi --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'

k get jobs -w

k logs pi--1-z6h8x

k delete jobs.batch pi

k create job busybox --image=busybox -- echo hello;sleep 30;echo world

k logs job/busybox

k delete jobs.batch busybox

k create job busybox --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'while true; echo hello; sleep 10; done;' > pod.yaml
```

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: busybox
spec:
  activeDeadlineSeconds: 30
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - while true; echo hello; sleep 10; done;
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: Never
status: {}
```

```shell
kubectl create job busybox --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'echo hello;sleep 30;echo world' > pod.yaml     
k get jobs.batch busybox -w      
```

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: busybox
spec:
  completions: 5
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - echo hello;sleep 30;echo world
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: Never
status: {}
```


```shell
k create cronjob busybox --image=busybox --schedule='*/1 * * * *' -- /bin/sh -c 'date;echo  hello'
k get cronjobs.batch busybox -w
k get pods --show-labels

k logs busybox-27379202--1-7628r

k delete cronjobs.batch busybox
```
```shell
k create cronjob busybox --image=busybox --schedule='*/1 * * * *' --dry-run=client -o yaml -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'  > pod.yaml
```

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  creationTimestamp: null
  name: busybox
spec:
  startingDeadlineSeconds: 17
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: busybox
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
            image: busybox
            name: busybox            resources: {}
          restartPolicy: OnFailure
  schedule: '*/1 * * * *'
status: {}
```