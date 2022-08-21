## Apply config & secrets -> db -> webapp (because of dependency)

`apply` manages applications through files defining k8s resources
```
kubectl apply -f <file-name.yaml>
```
to view configmaps,
```
kubectl describe configmaps
```
or
```
kubectl get configmap
kubectl get secret
```

## Monitoring with kubectl commands
get all pod, service, deployment, replicaset, ...
```
kubectl get all
```
get certain component as pod,
```
kubectl get pod
```

## Detail

for more detail about certain component,
```
kubectl describe <resourceType> <resourceName>
```
for example, service instance named `webapp-service`,
```
kubectl describe service webapp-service
```

## Logs of container
```
kubectl logs webapp-deployment-....
```
in window, we can use like 
```
kubectl logs $(kubectl get pod | findstr webapp | awk '{print $1}')
```
but of course, you need to install awk (maybe using chocolatery)

# Accessing
NodePort Service is accessible on each Worker Node's IP address

and since we're using minikube (1 machine), 

```
minikube ip
```

or using kubectl with production env, command `get node` with wide output will give us the ip.

```
kubectl get node -o wide
```

but in my case, this didn't work and from [hagrawal's answer](https://stackoverflow.com/questions/66607112/minikube-on-wsl2-windows-10-minikube-ip-not-reachable),

```
minikube service <YOUR_SERVICE_NAME>
```
did the right thing.
```
OUT:
|-----------|----------------|-------------|---------------------------|
| NAMESPACE |      NAME      | TARGET PORT |            URL            |
|-----------|----------------|-------------|---------------------------|
| default   | webapp-service |        3000 | http://192.168.49.2:30100 |
|-----------|----------------|-------------|---------------------------|
🏃  Starting tunnel for service webapp-service.
|-----------|----------------|-------------|-----------------------|
| NAMESPACE |      NAME      | TARGET PORT |          URL          |
|-----------|----------------|-------------|-----------------------|
| default   | webapp-service |             | http://127.0.0.1:9473 |
|-----------|----------------|-------------|-----------------------|
🎉  Opening service default/webapp-service in default browser...
❗  Because you are using a Docker driver on windows, the terminal needs to be open to run it.
```


# Restarting
[Link](https://linuxhint.com/kubectl-restart-the-pod/)
```
kubectl rollout restart deployment <deployment name>
```