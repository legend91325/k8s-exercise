### Defining and Querying Labels and Annotations

```shell
k run frontend --image=nginx --restart=Never --annotations="contact=John Doe" --annotations="commit=2d3mg3" --labels="env=prod,team=shiny"

k run backend --image=nginx --restart=Never --annotations="contact=Mary Harris" --labels="env=prod,team=legacy,app=v1.2.4"

k run database --image=nginx --restart=Never --labels="env=prod,team=storage"

k get pods --show-labels
k get pods -l  'team in (shiny,legacy)',env=prod --show-labels

k label pod backend env-

kubectl get pods -o yaml | grep -C 3 'annotations:'
```

### Performing Rolling Updates for a Deployment
```shell
k create deployment deploy --image=nginx --dry-run=client -o yaml > deploy.yaml

k get deployments.apps

k set image deployment/deploy nginx=nginx:latest

k rollout history deploy

k rollout history deployment --revision=2

k scale deployment deploy  --replicas=5

k rollout undo deployment/deploy --to-revision=1

k rollout history deployment

k rollout history deployment --revision=3
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    tier: backend
  name: deploy
spec:
  replicas: 3  
  selector:
    matchLabels:
      app: v1
  strategy: {}
  template:    
    metadata:      
      creationTimestamp: null      
      labels:        
        app: v1    
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```


### Creating a Scheduled Container Operation
```shell
k create cronjob current-date --schedule="* * * * *" --restart=OnFailure --image=nginx -- /bin/sh -C 'echo "Current date: $(date)"'

kubectl get jobs --watch

kubectl get pods --show-labels

k get cronjobs.batch -o yaml | grep successfulJobsHistoryLimit

k delete cronjobs.batch current-date
```