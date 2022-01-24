### Configuring a Pod to Use a ConfigMap

```shell
echo -e "DB_URL=localhost:3306\nDB_USERNMAE=postgres" > config.txt

k create configmap  db-config --from-evn-file=config.txt

k run backend --image=nginx --restart=Never -o yaml --dry-run > pod.yaml
# add configMap
envFrom: 
    - configMapRef:
        name: db-config

k apply -f pod.yaml

k exec -it backend -- /bin/sh
# env
```
### Configuring a Pod to Use a Secret

```shell
k create secret generic  db-credentials  --from-literal=db-password=passwd

k get sercerts

k run backend --image=nginx --restart=Never -o yaml --dry-run > pod.yaml
# add sercert env 
env: 
    - name: DB_PASSWORD
      valueFrom: 
        secretKeyRef: 
            name: db-credentials
            key: db-password


k apply -f pod.yaml 

k exec -it backend -- /bin/sh 

# env 
````

### Creating a Security Context for a Pod

```shell
k run secured --image=nginx --restart=Never -o yaml --dry-run > secured.yaml

# volume mount 
securityContext:
   fsGroup: 3000   
   ....
   volumneMounts: 
   - name:data-vol
     mountPath: /data/app
   ....
volumes: 
- name: data-vol
  emptyDir: {}   


k apply -f secured.yaml 

k exec -it secured -- /bin/sh 
# cd /data/app/
# touch logs.txt
#  ls -ls
```
### Defining a Pod’s Resource Requirements

```shell
k create namespace rq-demo
k create -f rq.yaml --namespace=rq-demo
k describe quota  --namespace=rq-demo
```

### Using a Service Account
```shell
k create serviceaccount backend-team 

k get serviceaccount backend-team -o yaml --export

k run backend --image=nginx --restart=Never --serviceaccount=backend-team

k exec -it backend -- /bin/sh 
#  cat /var/run/secrets/kubernetes.io/serviceaccount/token
```