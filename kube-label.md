
```
kubectl label node node1-name color=black for the master.
kubectl label node node2-name color=red for node 2.
kubectl label node node3-name color=green for node 3.
kubectl label node node4-name color=blue for node 4.
```
```
2. kubectl label pods -n default running=beforeLabels --all
```


3. alpine-label.yaml:

```
apiVersion: v1
kind: Pod
metadata:
  name: alpine
  namespace: default
  labels:
    running: afterLabels
spec:
  containers:
  - name: alpine
    image: alpine
    command:
      - sleep
      - "60"
  restartPolicy: Always
```


4. kubectl get pods -l running=beforeLabels -n default

5. kubectl label pods --all -n default tier=linuxAcademyCloud

6. kubectl get pods -l running=afterLabels,tier=linuxAcademyCloud
