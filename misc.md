# Manual scheduling
In the pod definition, set the nodeName: in the spec to manually schedule the pod on the node.
This could not be changed, unless we use a binding objet applyied with a post on the API

# Label and selectors
Labels are a way to filter objects
The filter are done with selector
ie : app=redis

## Labels in pods
In the yaml file, in the metadata section, add :
metadata:
   labels:
      app:app-name
      env:prod

To get the pod, once it has been created, use the --selector env=prod to list the desired port

## Labels elsewhere
On replicaSet, there are 2 places where labels are defined : one for the replicaSet itself, and one for the pods defined 
in the template. In the spec of replicaSet, the selector should match the labels defined on the pods.

On services, it quite the same ; selector in service, match pods defined somewhere else

## Some exemple
kubectl get pods --selector env=prod,bu=finance,tier=frontend
kubectl get all --selector env=prod

# Taints and Tolerations 
Taints are set on node
Tolerations are set on pod
Pods can be scheduled on node only if it has a toleration for the taint of the node
BUT pods can still be scheduled on another node without taint
Taint and Toleration doesn t garantee the pod to be scheduled on a particular node. It garantees that untolerant pods
can t be scheduled on a tainted node.

## Taint a node
kubectl taint nodes name-of-the-node disk=ssd:taint-effect
Here the node is tainted with "ssd" (key is disk)
What happens to pod that do not tolerate the taint ?
3 possibilities (taint-effect)
 - NoSchedule : if the taint does not match the toleration, the pod will not be scheduled 
 - PreferNoSchedule : The scheduler will try to avoid the node, but is is not garanteed 
 - NoExecute : If pods are already scheduled, and node is tainted, untolerant pods will be killed

 Masternodes are tainted with NoSchedule taint effect

 # Node selector
 Allow pod to be scheduled on particular node ;
 Label is applyed to node, and new attribute in spec section called nodeSelector muste be created 
 with the key=value that match the previous label

Set the label on node :
kubectl label nodes nameOfNode size=big

In the pod definition, add this under spec
nodeSelector:
 size: big

 WARNING : with nodeSelector can t be complex : only on key ; you can t set size: big or size: medium or NOT size: small

 See next section for that

 # Node Affinity
Scheduled will place the pods on desired node
NodeAffinity have quite complexe keys ;
There is 2 keys that act on pods scheduling, and once the pod is running
requiredDuringSchedulingIgnoredDuringExecution
preferedDuringSchedulingIgnoredDuringExecution

See exemple below :
under spec:
affinity:
 nodeAffinity:
   requiredDuringSchedulingIgnoredDuringExecution:
    nodeSelectorTerms:
     - matchExpressions:
      - key: color
        operator: Exists
 