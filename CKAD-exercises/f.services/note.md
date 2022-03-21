```shell
k run nginx --image=nginx --restart=Never --port=80 --expose

k get service

k get endpoints

k run busybox --rm --image=busybox -it  --restart=Never -- /bin/sh -c 'wget -O- 10.36.1.161:80'
```

```shell
kubectl create deploy foo --image=dgkanatsios/simpleapp --port=8080 --replicas=3

k label deployments.apps foo app=foo --overwrite=true

k get pod -l app=foo -o wide

k run busybox --image=busybox --rm -it --restart=Never -- /bin/sh -c 'wget -O- '

k expose deployment foo --port=6262 --target-port=8080

k get services foo

k get endpoints foo
```

```shell
kubectl run busybox --image=busybox -it --rm --restart=Never -- sh

wget -O- foo:6262
```

```shell
k create deployment nginx --image=nginx --replicas=2 --port=80
# 测试的时候没有生效 因为我使用谷歌云 默认网络策略没有开
k run busybox --image=busybox --rm --it --restart=Never -- wget -O- http://nginx:80 --timeout 2
k run busybox --image=busybox --rm -it --restart=Never --labels=access=granted -- wget -O- http://nginx:80 --timeout 2
```

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-nginx # pick a name
spec:
  podSelector:
    matchLabels:
      app: nginx # selector for the pods
  ingress: # allow ingress traffic
  - from:
    - podSelector: # from pods
        matchLabels: # with this label
          access: granted
```