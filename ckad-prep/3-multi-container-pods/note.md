### Implementing the Adapter Pattern
```shell

kubectl run adapter --image=busybox --restart=Never -o yaml --dry-run -- /bin/sh -c 'while true; do echo "$(date) | $(du -sh ~)" >> /var/logs/diskspace.txt; sleep 5; done;' > adapter.yaml

k exec -it adapter --container=transformer -- /bin/sh
# ls -l
# cat 2022-01-16-15-48-12-transformed.txt
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: adapter
  name: adapter
spec:
  volumes: 
    - name: config-volume
      emptyDir: {}
  containers:
  - args:
    - /bin/sh
    - -c
    - while true; do echo "$(date) | $(du -sh ~)" >> /var/logs/diskspace.txt; sleep
      5; done;
    image: busybox
    name: adapter
    volumeMounts: 
      - name: config-volume
        mountPath: /var/logs
    resources: {}
  - image: busybox
    name: transformer
    args: 
    - /bin/sh
    - -c 
    - 'sleep 20; while true; do while read LINE; do echo "$LINE" | cut -f2 -d"|" >> $(date +%Y-%m-%d-%H-%M-%S)-transformed.txt; done < /var/logs/diskspace.txt; sleep 20; done;'
    volumeMounts: 
      - name: config-volume
        mountPath: /var/logs
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```