# ConfigMap

Used to store data, injected in container

first create the config map

## Imperative :
kubectl create configmap

### with variable from command line (--from-literal)

IE : kubectl create configmap my-configmap --from-literal=ENV=prod --from-literal=COLOR=blue

Here 2 env variable would be created from command line

### with variable from a file (--from-file)
IE : kubectl create configmap my-configmap --from-file=/path/to/file

File contains :
ENV: prod
COLOR: red

## Declarative:
kubectl create -f configmapdef.yaml

then reference it in container spec

IE : configmapdef.yaml

apiVersion: v1
kind: ConfigMap
metaData:
 name: config-map-test
data:
 ENV: prod
 COLOR: red

## View config Map

kubectl get configmaps
kubectl describe configmap my-configmap

# secret
Same as configmap but value are hashed

## imperative

### with variable from command line (--from-literal)

kubectl create secret generic

IE :
kubectl create secret generic --from-literal=KEY=value
### with variable from command file (--from-file)


kubectl create secret generic

IE :
kubectl create secret generic --from-file=/path/to/file
## declarative

kubectl create- f my-secret.yaml

apiVersion: v1
kind: Secret
metadata:
  name:  secretName
data:
   secretKey:  BASE64_ENCODED_VALUE


The encoded version of value are got from this kind of command :
echo -e 'password' | base64

The secret works the same as configmap.

# Note about secret

Remember that secrets encode data in base64 format. Anyone with the base64 encoded secret can easily decode it. As such the secrets can be considered as not very safe.

The concept of safety of the Secrets is a bit confusing in Kubernetes. The kubernetes documentation page and a lot of blogs out there refer to secrets as a "safer option" to store sensitive data. They are safer than storing in plain text as they reduce the risk of accidentally exposing passwords and other sensitive data. In my opinion it's not the secret itself that is safe, it is the practices around it. 

Secrets are not encrypted, so it is not safer in that sense. However, some best practices around using secrets make it safer. As in best practices like:

    Not checking-in secret object definition files to source code repositories.

    Enabling Encryption at Rest for Secrets so they are stored encrypted in ETCD.  


Also the way kubernetes handles secrets. Such as:

    A secret is only sent to a node if a pod on that node requires it.

    Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.

    Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the protections and risks of using secrets here


Having said that, there are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, HashiCorp Vault. 