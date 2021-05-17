## Presto on Kubernetes

## Install

1. Clone this repo:

`git clone https://github.com/asifkazi/presto-on-kubernetes.git`

2. Create a K8S namespace for your cluster:

`kubectl create namespace presto`

3. Apply the persistent volume and mysql deployment 
 `kubectl apply -f ./yaml/mysql-deployment.yaml --namespace presto`
 `kubectl apply -f ./mysql-deployment.yaml --namespace presto`

4. Make sure the pod came up running
  `kubectl get pods -n presto`
    
  Example:

  `kubectl get pods -n default
    NAME                     READY   STATUS    RESTARTS   AGE
    mysql-5477d96fbf-24r7p   1/1     Running   0          6s`

5. Verify connectivity / working database
   `kubectl run -it --rm --image=mysql:5.7 --restart=Never mysql-client -- mysql -h mysql -udbuser -pdbuser`

    If you don't see a command prompt, try pressing enter.

    `mysql> show databases;`
    ```+--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | demodb             |
    +--------------------+
    2 rows in set (0.00 sec)```

6. Quit out of the client
    ```mysql> exit
    Bye
    pod "mysql-client" deleted```

7. Install Presto Coordinator and Workers in your K8s cluster:

`kubectl apply -f presto.yaml --namespace presto`

## Changing configurations in the Cluster

1. Change the configuration file

2. Apply the change to the cluster

`kubectl apply -f presto.yaml --namespace presto`

## Uninstall

Uninstall Presto cluster in your K8s cluster:

* `kubectl delete -f presto.yaml --namespace presto`
* `kubectl delete deployment,svc mysql --namespace presto`
* `kubectl delete pvc mysql-pv-claim --namespace presto`
* `kubectl delete pv mysql-pv-volume --namespace presto`
