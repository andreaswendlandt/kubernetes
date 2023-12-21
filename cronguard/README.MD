# CRONGUARD - K8S
## running all containers from the cronguard application based on these images and setup https://github.com/andreaswendlandt/docker/tree/master/cronguard in a k8s cluster
##

### these 5 manifest files are necessary
* cronguard-daemon-deployment.yaml
* cronguard-db-deployment.yaml
* cronguard-db-service.yaml
* cronguard-web-deployment.yaml
* cronguard-web-service.yaml

### create all pods, deployments and services
```
kubectl create -f cronguard-daemon-deployment.yaml -f cronguard-db-deployment.yaml -f cronguard-db-service.yaml -f cronguard-web-deployment.yaml -f cronguard-web-service.yaml

deployment.apps/cronguard-daemon created
deployment.apps/cronguard-db created
service/cronguard-db created
deployment.apps/cronguard-web created
service/cronguard-web created

```

### check that everything is up and running
```
kubectl get services

NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
cronguard-db    NodePort    10.110.106.247   <none>        3306:32337/TCP   44s
cronguard-web   NodePort    10.99.34.82      <none>        8080:31322/TCP   44s
kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP          11d

kubectl get pods

NAME                                READY   STATUS    RESTARTS   AGE
cronguard-daemon-7b767db6cf-mr2r7   1/1     Running   0          89s
cronguard-db-855d6597f7-42gkf       1/1     Running   0          89s
cronguard-web-97695b778-wbrq2       1/1     Running   0          89s

kubectl get deployments.apps

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
cronguard-daemon   1/1     1            1           119s
cronguard-db       1/1     1            1           119s
cronguard-web      1/1     1            1           119s
```

### get the external url - for testing purposes i used minikube as a k8s "cluster"
```
minikube service cronguard-web --url

http://192.168.49.2:31322
```

### delete everything afterwards
```
kubectl delete -f cronguard-db-service.yaml -f cronguard-daemon-deployment.yaml -f cronguard-db-deployment.yaml -f cronguard-web-deployment.yaml -f cronguard-web-service.yaml

deployment.apps "cronguard-daemon" deleted
deployment.apps "cronguard-db" deleted
deployment.apps "cronguard-web" deleted
service "cronguard-web" deleted
```

