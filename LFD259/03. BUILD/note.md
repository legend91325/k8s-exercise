readinessProbes --> accepting traffic from the cluster
livenessProbes --> ensure that the container and the application continues to run in a helathy state
startupProbe --> disable liveness and readiness check unitl the application passes the test

for name in  $(k get pod -l app=try1 -o name) 
   do 
   k exec $name -c simpleapp  -- touch /tmp/healthy 
   done


1.  Using the three URL locations allowed by the exam, find and bookmark working YAML examples for LivenessProbes,ReadinessProbes, and multi-container pods.
2.  Deploy a new nginx webserver. Add a LivenessProbe and a ReadinessProbe on port 80. Test that both probes and thewebserver work.
3.  Use thebuild-review1.yamlfile to create a non-working deployment.  Fix the deployment such that both containersare running and in aREADYstate. The web server listens on port 80, and the proxy listens on port 8080.
4.  View the default page of the web server. When successful verify theGETactivity logs in the container log. The messageshould look something like the following. Your time and IP may be different.192.168.124.0 - - [30/Jan/2020:03:30:31 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.58.0" "-"
5.  Remove any resources created in this review.V2021-12-09© Copyright the Linux Foundation 2022. All rights reserved.
