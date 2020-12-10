# Kubernetes Probes

## Understanding Kubernetes Probes

All the probe have the following parameters:

* **initialDelaySeconds** : number of seconds to wait before initiating liveness or readiness probes
* **periodSeconds**: how often to check the probe
* **timeoutSeconds**: number of seconds before marking the probe as timing out (failing the health check)
* **successThreshold** : minimum number of consecutive successful checks for the probe to pass
* **failureThreshold** : number of retries before marking the probe as failed. For liveness probes, this will lead to the pod restarting. For readiness probes, this will mark the pod as unready.

### **Readiness Probes**
**Readiness probes are used to let kubelet know when the application is ready to accept new traffic.** If the application needs some time to initialize state after the process has started, configure the readiness probe to tell Kubernetes to wait before sending new traffic. A primary use case for readiness probes is directing traffic to deployments behind a service.

![zoomify](imgs/readness_probe.gif)<br></br>
***Reference: GCP Blog***

***Readiness probes is that it runs during the pod’s entire lifecycle.*** This means that readiness probes will run not only at startup but repeatedly throughout as long as the pod is running. This is to deal with situations where the application is temporarily unavailable (i.e. loading large data, waiting on external connections). In this case, we don’t want to necessarily kill the application but wait for it to recover. Readiness probes are used to detect this scenario and not send traffic to these pods until it passes the readiness check again.

### **Liveness Probes**
***Liveness probes are used to restart unhealthy containers.*** The kubelet periodically pings the liveness probe, determines the health, and kills the pod if it fails the liveness check. Liveness checks can help the application recover from a deadlock situation. Without liveness checks, Kubernetes deems a deadlocked pod healthy since the underlying process continues to run from Kubernetes’s perspective. By configuring the liveness probe, the kubelet can detect that the application is in a bad state and restarts the pod to restore availability.

![zoomify](imgs/liveness_probe.gif)<br></br>
***Reference: GCP Blog & Image Credit: GCP Blog***

### **Startup Probes**

***Startup probes are similar to readiness probes but only executed at startup.*** They are optimized for slow starting containers or applications with unpredictable initialization processes.