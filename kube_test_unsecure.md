Our Kubernetes cluster has been configured to run untrusted workloads under a more secure configuration using runsc. In this lesson, we will make sure that this functionality is working correctly. We will create a pod that is marked as untrusted, then we will log in to the worker node and dig into the runsc data to make sure that the container is actually running under runsc. This will verify that our cluster is running untrusted workloads with the correct configuration on the worker node.

For this lesson, you will need to connect to cluster using kubectl. You can log in to one of your controller server and use it there, or you can use it from your local machine. To use kubectl from your local machine, you will need to open an SSH tunnel. You can open the SSH tunnel by running this in a separate terminal. Leave the session open while you are working to keep the tunnel active:

```
ssh -L 6443:localhost:6443 user@<your Load balancer cloud server public IP>
```

First, create an untrusted pod:

```
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: untrusted
  annotations:
    io.kubernetes.cri.untrusted-workload: "true"
spec:
  containers:
    - name: webserver
      image: gcr.io/hightowerlabs/helloworld:2.0.0
EOF
```

Make sure that the untrusted pod is running:

```
kubectl get pods untrusted -o wide
```

Take note of which worker node the untrusted pod is running on, then log into that worker node.

On the worker node, list all of the containers running under gVisor:

```
sudo runsc --root  /run/containerd/runsc/k8s.io list
```

Get the pod ID of the untrusted pod and store it in an environment variable:

```
POD_ID=$(sudo crictl -r unix:///var/run/containerd/containerd.sock \
  pods --name untrusted -q)
```

Get the container ID of the container running in the untrusted pod and store it in an environment variable:

```
CONTAINER_ID=$(sudo crictl -r unix:///var/run/containerd/containerd.sock \
  ps -p ${POD_ID} -q)
```

Get information about the process running in the container:

```
sudo runsc --root /run/containerd/runsc/k8s.io ps ${CONTAINER_ID}
```

Since we were able to get the process info using runsc, we know that the untrusted container is running securely as expected.
