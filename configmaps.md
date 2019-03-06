Creating a config map

```
>kubectl create configmap my-map --from-literal=myKey=myValue
>kubectl get configmaps
```

Use config map in yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: my-configmap-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo $(MY_VAR) && sleep 3600"]
    env:
    - name: MY_VAR
      valueFrom:
        configMapKeyRef:
          name: my-map
          key: myKey
```
