
# Create directory as the local mount
mkdir -p /tmp/pvc-local-path

# Deploy the pvc.yaml



root@sample:/opt/dev/pvc_example# kubectl get pvc -A
NAMESPACE   NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       VOLUMEATTRIBUTESCLASS   AGE
hello-pvc   hello-pvc            Bound    hello-pv                                   2Gi        RWO            hello-local-path   <unset>                 6m8s
root@cnx:/opt/dev/pvc_example#



root@sample:/opt/dev/pvc_example# kubectl get pv -A
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                       STORAGECLASS       VOLUMEATTRIBUTESCLASS   REASON   AGE
hello-pv                                   2Gi        RWO            Delete           Bound    hello-pvc/hello-pvc         hello-local-path   <unset>                          6m21s



root@sample:/opt/dev/pvc_example# kubectl get storageclass -A
NAME                   PROVISIONER                       RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
hello-local-path       kubernetes.io/minikube-hostpath   Delete          WaitForFirstConsumer   false                  6m36s


# Deploy the pod




root@sample:/opt/dev/pvc_example# kubectl get all -n hello-pvc
NAME                                    READY   STATUS    RESTARTS   AGE
pod/hello-client-k8s-657dcd5d4d-lz48z   1/1     Running   0          7m46s
pod/hello-client-k8s-657dcd5d4d-mbmfz   1/1     Running   0          8s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-client-k8s   2/2     2            2           7m46s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-client-k8s-657dcd5d4d   2         2         2       7m46s



root@cnx:/opt/dev/pvc_example# kubectl exec -it -n hello-pvc hello-client-k8s-657dcd5d4d-lz48z -- /bin/bash
root@hello-client-k8s-657dcd5d4d-lz48z:/#
root@hello-client-k8s-657dcd5d4d-lz48z:/#
root@hello-client-k8s-657dcd5d4d-lz48z:/#
root@hello-client-k8s-657dcd5d4d-lz48z:/# cd /var/log/app/delivery
root@hello-client-k8s-657dcd5d4d-lz48z:/var/log/app/delivery#
root@hello-client-k8s-657dcd5d4d-lz48z:/var/log/app/delivery#
root@hello-client-k8s-657dcd5d4d-lz48z:/var/log/app/delivery# echo "Hello Nuwas" > nuwas.txt
root@hello-client-k8s-657dcd5d4d-lz48z:/var/log/app/delivery#
root@hello-client-k8s-657dcd5d4d-lz48z:/var/log/app/delivery#
root@hello-client-k8s-657dcd5d4d-lz48z:/var/log/app/delivery# exit


root@sample:/opt/dev/pvc_example# kubectl exec -it -n hello-pvc hello-client-k8s-657dcd5d4d-mbmfz -- /bin/bash
root@hello-client-k8s-657dcd5d4d-mbmfz:/#
root@hello-client-k8s-657dcd5d4d-mbmfz:/#
root@hello-client-k8s-657dcd5d4d-mbmfz:/# cd /var/log/app/delivery
root@hello-client-k8s-657dcd5d4d-mbmfz:/var/log/app/delivery#
root@hello-client-k8s-657dcd5d4d-mbmfz:/var/log/app/delivery#
root@hello-client-k8s-657dcd5d4d-mbmfz:/var/log/app/delivery# cat nuwas.txt
Hello Nuwas
root@hello-client-k8s-657dcd5d4d-mbmfz:/var/log/app/delivery# exit


root@sample:/opt/dev/pvc_example# ls /tmp/pvc-local-path
nuwas.txt
root@sample:/opt/dev/pvc_example#

