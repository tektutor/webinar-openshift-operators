# webinar-openshift-operators
## Services supported by Kubernetes/Openshift
<pre>
- ClusterIP Service - For internal access ( Pods running within the same Openshift cluster can access )  
- NodePort Service - For external access ( end-users or applications running outside Openshift cluster can access )
- LoadBalancer Service - For external access ( used in public cloud environment like AWS, Azure, GCP, etc.,)
</pre>

## How NodePort services are accessed
<pre>
curl http://node-ip:node-port
</pre>

## Openshift Route
<pre>
- Openshift added a new feature called Routes ( based on Ingress )
- exposes the service for external access ( outside the cluster )  
</pre>

## What happens when we deploy an application into Openshift
When we issue the below command
```
oc new-app bitnami/nginx:latest
```
These are the activities that happens internally within openshift
<pre>
- oc openshift client makes a REST call to API Server, requesting it to create deployment nginx
- API Server receives the request, it then creates a nginx deployment datbase record in etcd database
- API Server sends a broadcasting event indicating a new deployment by name nginx is created
- Deployment Controller receives this event, it then makes a REST call to API Server requesting it to create a replicaset for the nginx deployment
- API Server creates a replicaset database entry in etcd database and its sends broadcasting event indicating a new ReplicaSet resource is created
- ReplicaSet controller receives this event, it then sends REST call to API Server to create number of Pods as defined in the replicaset
- API servers creates the Pod record in etcd database and it sends broadcasting events saying new Pod created
- Scheduler receives the event, it identifies a healthy node where the new Pod can be deployed, it sends the scheduling recommendation to API Server via REST call
- API Server will retrieve the corresponding Pod record from etcd database and then it updates the scheduling details that came from scheduler
- API Server then sends broadcasting events saying nginx pod scheduled on to so and so node
- the kubelet container agents that runs in that particular node receives the event, it then gets to know what container image must be used to the create Pod containers, it pulls the container image, it creates the containers, starts the container and reports the status back to API server via REST call
</pre>  

## Custom Resources
<pre>
- Let's say we wish to add a new type of resource called Training into Openshift cluster, then we need to create a Custom Resource called Training
- Custom Resources can added into Kubernetes/Openshift via Custom Resource Definitions
- Custom Resource Definition(CRD) are defined as a YAML file
</pre>

## Custom Controllers
<pre>
- 
</pre>

## Openshift Operator Overview
<pre>
- helps in extending Openshift API( features)
- we can add additional functionalities to Openshift via Operators
- Operators is a combination of Custom Resources and Custom Controllers
- Operator is a special package that combines the Custom Resources and Custom Controllers in a way Openshift will allow us to install into Openshift Orchestration Platform

</pre>
