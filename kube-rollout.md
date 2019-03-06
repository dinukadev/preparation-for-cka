Create the yaml file and name it something. I chose nginx-deployment.yaml. Create the deployment object by calling:
```
kubectl create -f nginx-deployment.yaml
```

There are many ways to get this, here's one example that gives you the results in one step and uses a label selector:

```
kubectl get pods -l app=nginx -o wide
```

There are many ways. Here are two:
This will work just fine but is not the preferredmethodbecausenowtheyaml is inconsistent with what you've got running in the cluster. Anyonecomingacrossyouryaml will assume it's what is up and running and it isn't.

```
kubectl set image deployment nginx-deployment nginx=nginx:1.8
```

Updatethelineintheyaml to the 1.8 version of the image, and apply the changes with
```
kubectl apply -f nginx-deployment.yaml
```
Same as above. Don't forget you can watch the status of the rollout with the command

```
kubectl rollout status deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment
```
will undo the previous rollout, or if you want to go to a specific point in history, you can view the history and roll back to a specific state with:
```
kubectl rollout history deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment --to-revision=x
kubectl delete -f nginx-deployment.yaml
```
