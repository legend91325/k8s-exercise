```shell
k create namespace mynamespace
k run nginx --image=nginx --restart=Never --namespace=mynamespace
k delete pod nginx --namespace=mynamespace
k run nginx --image=nginx --restart=Never --namespace=mynamespace --dry-run=client -o yaml > pod.yaml
k apply -f pod.yaml
# nice command
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml | kubectl create -n mynamespace -f -

```

```shell
kubectl run busybox --image=busybox --command --restart=Never -it -- env 
k run busybox --image=busybox --restart=Never --rm -it -- /bin/sh -c 'env'
k create namespace myns -o yaml --dry-run=client
k create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run=client -o yaml > myrq.yaml
k get pod --all-namespaces
k run nginx --image=nginx --restart=Never --port=80
```

```shell
k set image pod/nginx nginx=nginx:1.7.1
k describe pod nginx 
```
```shell
k get pod nginx -o wide
# NAME    READY   STATUS    RESTARTS        AGE     IP           NODE                                 NOMINATED NODE   READINESS GATES
# nginx   1/1     Running   1 (2m15s ago)   4m12s   10.36.2.14   gke-gke-default-pool-b59a111e-8hq6   <none>           <none>
k run busybox --image=busybox --restart=Never --rm -it -- /bin/sh -c 'wget -O- 10.36.2.14:80/'
```

```shell
k get pod nginx -o yaml
k logs nginx
k logs nginx --previous
```

```shell
k run nginx --image=nginx --restart=Never --env=var1=val1 -it --rm -- env
```