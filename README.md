# k8s-exercise
k8s exercise

### 必要信息
[cncf-ckad]https://www.cncf.io/certification/ckad/
[faq-ckad](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks)
[tips-ckad](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
[注册考试码](https://training.linuxfoundation.cn/help/10)

### 免费学习资料
[CKAD-exercises](https://github.com/dgkanatsios/CKAD-exercises)
[ckad-prep](https://github.com/bmuschko/ckad-prep)

```shell
# https://www.wiic.top/05/vim%E5%86%99yaml%E6%96%87%E4%BB%B6%E7%9A%84%E5%8F%82%E6%95%B0/
vim ~/.vimrc
autocmd FileType yaml setlocal et ai ts=2 sw=2
```

```shell
kubectl explain pod.spec.containers.livenessProbe
```
```shell
echo -n YWRtaW4= | base64 -d
```

```shell
# killer.sh suggest
alias k=kubectl                         # will already be pre-configured

export do="--dry-run=client -o yaml"    # k get pod x $do

export now="--force --grace-period 0"   # k delete pod x $now


grep -i 
kubectl -n 
base64 -d
grep -A2
```

```shell
vim ~/.vimrc

set tabstop=2
set expandtab
set shiftwidth=2
```
> To indent multiple lines using vim you should set the shiftwidth using :set shiftwidth=2. Then mark multiple lines using Shift v and the up/down keys.
To then indent the marked lines press > or < and to repeat the action press .


```kubectl 
k -n pluto expose pod -h # help

k -n pluto expose pod project-plt-6cc-api --name project-plt-6cc-svc --port 3333 --target-port 80

k -n pluto delete pod holy-api --force --grace-period=0
```


```docker
sudo docker build -t registry.killer.sh:5000/sun-cipher:latest -t registry.killer.sh:5000/sun-cipher:v1-docker .

sudo docker image ls

sudo docker push registry.killer.sh:5000/sun-cipher:latest

sudo docker push registry.killer.sh:5000/sun-cipher:v1-docker
```

```podman
podman build -t registry.killer.sh:5000/sun-cipher:v1-podman .

podman image ls

podman push registry.killer.sh:5000/sun-cipher:v1-podman

podman run -d --name sun-cipher registry.killer.sh:5000/sun-cipher:v1-podman

podman ps

podman ps > /opt/course/11/containers

podman logs sun-cipher > /opt/course/11/logspodman logs sun-cipher
```

```kubectl
k -f /opt/course/14/secret-handler.yaml delete --force --grace-period=0
k -f /opt/course/14/secret-handler-new.yaml create

k -f /opt/course/14/secret-handler-new.yaml replace --force --grace-period=0
```

```kubectl
k -n moon rollout restart deploy web-moon

# specify container name in pod
k -n mercury logs cleaner-576967576c-cqtgx -c logger-con

# sh -c 
```

> Though Pods are usually never created without a Deployment or ReplicaSet, Services always select for Pods directly. This gives great flexibility because Pods could be created through various customized ways. 

```shell
wget -O- -T 5 www.google.de:80 
```

