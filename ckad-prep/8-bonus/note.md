### Shortcuts and Time Savers
```shell

source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.

alias k=kubectl
complete -F __start_kubectl k
```

```shell
k config set-context <context-of-question> --namespace=<namespace-of-question>
# use context
k config use-context <context-of-question>
```

```shell
k delete pod nginx --grace-period=0 --force
```

### Bash Commands
```shell
if [! -d ~/tmp]; then mkdir -p ~/tmp; fi; while true; do echo $(date) >> ~/tmp/date.txt; sleep 5; done;
```
```shell
counter=0; while true; do counter=$((counter+1)); echo "$count"; sleep 1; done;
```
```shell
while true; do random=$((RNDOM % 100) + 1); if[ $random -le 50]; then echo "$random"; else echo "END: $randome"; break; fi; sleep 1; done;
```

### Kubernetes Object Information
```shell
k describe pods -o yaml | grep -C 10 'author=John Doe'
```

```shell
kubectl get pods -o yaml | grep -C 5 labels:
```