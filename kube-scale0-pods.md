To scale the deployment up to 4 pods, use: kubectl scale deployment nginx-deployment --replicas=4

To make it so 3 pods can be available and apply the changes to the existing deployment, complete the following:

Edit the YAML as follows:
```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
Execute the command: kubectl apply -f nginx-deployment.yml 
