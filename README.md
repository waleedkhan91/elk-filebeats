# ELK stack POC for nginx logs using filebeat.

This is a complete Demo POC for ELK stack implementation using filebeats on single Master Kubernetes cluster.

User can modify this demonstration as per their needs. Basic flow of this deployment is the running instance of nginx server which generates access and error logs. We will process these logs into the ELK stack using filebeats.

For this implementation, I have used hostPath volume mount. Nginx and Filebeats pods are forcefully run on single node. Please note that this is not authentic approach for production systems. We have to implement proper volume mounting strategy like `fc` or `nfs`.

First we assign the label to one of the worker node before rolling out the deployment. I will assume that the node name of my worker node is node-02. You can get the node names by `kubectl get nodes` command.

> kubectl label nodes node-02 test=filebeats

After the successfull assignment of node label, we start deployment in following order.
```
kubectl apply -f elk-fb-configmap.yaml
```
```
kubectl apply -f nginx.yaml
```
```
kubectl apply -f elasticsearch.yaml
```
Wait for few minutes to successfully start the elasticsearch. Then proceed with:
```
kubectl apply -f logstash.yaml
```
```
kubectl apply -f kibana.yaml
```
```
kubectl apply -f filebeat.yaml
```
verify all the pods are running by:
```
kubectl get pods
```
After successfull installation verify the flow by generating get requests to nginx and then check the logs in kibana. You manually have to add index pattern in kibana before the logs are appeared.

Feel free to drop me an email if you have any questions.

Cheers !!!

###### *****************************
###### Author: Waleed Khan
###### email: waleedkhan91@gmail.com
###### *****************************
