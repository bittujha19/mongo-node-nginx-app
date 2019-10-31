TASK :
=====

This task assumes that you have a minikube cluster ready and helm installed and running on your local Machine, 
but this application can be run on any Cloud Provider.


#1 Please follow Below steps exactly to get this working on a Minikube Cluster:

#git clone https://github.com/bittujha19/mongo-node-nginx-app.git
#cd mongo-node-nginx-app
#helm install --name mongodb mongodb/
#helm install --name nginx-ingress nginx-ingress/
#helm install --name nodeapp nodeapp/

#### Running below command will output service endpoint details:

admins-MacBook-Pro-4:mongo-node-nginx-app bittujha$ Kubectl get svc
NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
kubernetes                      ClusterIP   10.96.0.1       <none>        443/TCP                      29d
mongodb                         ClusterIP   10.101.111.5    <none>        27017/TCP                    9m55s
nginx-ingress-controller        NodePort    10.111.105.45   <none>        80:32753/TCP,443:30642/TCP   24s
nginx-ingress-default-backend   ClusterIP   10.106.6.176    <none>        80/TCP                       24s
nodeapp                         ClusterIP   10.107.106.64   <none>        3000/TCP                     8m59s


#minikube ip (In my case it's 192.168.99.101, find yours by executing the command)

# Now Hit the below url on browser:
http://192.168.99.101:32753/

Response: {"status_code":404,"message":"Not found"}

Now try:
http://192.168.99.101:32753/users

Response: {"data":[],"meta":{"count":0}}

Now execute Below Curl Commands (I used postman to cretae this, you can try anything) :

curl -X POST \
  http://192.168.99.101:32753/users \
  -H 'authorization: Basic cm9vdDpwYXNzd29yZA==' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: ef331ef9-f61a-4690-ffc3-bf94072df97f' \
  -d '{
	"name": "database_alpha"
}'

Now try Again:
http://192.168.99.101:32753/users

You will receive below output.

{"data":[{"_id":"5dbaa7e69281900018b928a9","name":"database_alpha","created_at":"2019-10-31T09:22:46.431Z"}],"meta":{"count":1}}




Below commands to check secrets and pvc:
=======================================

admins-MacBook-Pro-4:mongo-node-nginx-app bittujha$ kubectl get pvc
NAME      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mongodb   Bound    pvc-012810ce-65b6-46ac-b8e8-bfadacbae526   1Gi        RWO            standard       18m


admins-MacBook-Pro-4:mongo-node-nginx-app bittujha$ kubectl get secrets
NAME                                TYPE                                  DATA   AGE
default-token-h8tmp                 kubernetes.io/service-account-token   3      29d
mongodb                             Opaque                                7      18m
nginx-ingress-backend-token-kspdx   kubernetes.io/service-account-token   3      9m24s
nginx-ingress-token-gpgvb           kubernetes.io/service-account-token   3      9m24s
admins-MacBook-Pro-4:mongo-node-nginx-app bittujha$



                                        ***************************
                                        Cheers we are all Done !!!!
                                        ***************************

