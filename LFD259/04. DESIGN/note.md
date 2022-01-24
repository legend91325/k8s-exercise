- Define an application's resource requirements.
- Understand multi-container Pod design patterns: sidecar, adapter, ambassador.​
- Discuss application design concepts.

Using Label Selectors

Sidecar
Adapter
Ambassador
init Containers 

As we have learned, the decoupled nature of Kubernetes depends on a collection of watcher loops, or operators, interrogating the kube-apiserver to determine if a particular configuration is true

CNI 
https://github.com/containernetworking/cni

Project Calico
https://docs.projectcalico.org/v3.0/introduction/
•Calico with Canal
https://docs.projectcalico.org/v3.0/getting-started/kubernetes/installation/hosted/canal
•Weave Works
https://www.weave.works/docs/net/latest/kubernetes/kube-addon
•Flannel
https://github.com/coreos/flannel•Ciliumhttp://cilium.io/V2021-12-09© Copyright the Linux Foundation 2022. All rights reserved.
- [Kube Router](https://www.kube-router.io3) 


Plugin Answers1.  Which of the plugins allow vxlans?Canal, Project Calico, Flannel, Weave Net, Cilium2.  Which are layer 2 plugins?Canal, Flannel, Weave Net3.  Which are layer 3?Project Calico, Romana, Kube Router4.  Which allow network policies?Project Calico, Canal, Kube Router, Weave Net, Cilium5.  Which can encrypt all TCP and UDP traffic?Project Calico, Weave Net, CiliumMulti Pod Answers1.  Which deployment method would allow the most flexibility, multiple applications per pod or one per Pod?One per pod2.  Which deployment method allows for the most granular scalability?One per pod3.  Which have the best inter-container performance?Multiple per pod.4.  How many IP addresses are assigned per pod?One5.  What are some ways containers can communicate within the same pod?IPC, loopback or shared filesystem access.6.  What are some reasons you should have multiple containers per pod?Lean containers may not have functionality like logging.  Able to maintain lean execution but add functionalityas necessary, like Ambassadors and Sidecar containers.V2021-12-09© Copyright the Linux Foundation 2022. All rights reserved.
