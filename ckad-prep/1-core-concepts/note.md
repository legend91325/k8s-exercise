```shell
k create namespace ckad-prep
```

```shell
k run mypod --image=nginx:2.3.4 --restart=Never --port 80 --namespace ckad-prep
```

```shell
k exec -it mypod -- /bin/sh --namespace ckad-prep
touch pod-error.txt 
exit
```

```shell
k edit pod mypod --namespace ckad-prep
# set nginx:1.15.12
```

```shell
k get pod  --namespace ckad-prep

k describe pdo -namespace ckad-prep
```

```shell
k exec mypod -it   --namespace ckad-pre  -- /bin/shp
ls 
exit
```

```shell
k get pod mypod -o wide --namespace ckad-prep
```

```shell
k run busybox --image=busybox --rm --it --restart=Never --namespace ckad-prep -- /bin/sh 
wget -O- $IP
```

```shell
k logs  mypod --namespace ckad-prep
```

```shell
k delete pod mypod --namespace ckad-prep
```

```shell
k delete namespace ckad-prep
```