use
command: ["",""]
args: ["",""]

in container  definition to overwrite the container commands and parameter ;

entrypoint is "mapped" to command
CMD is mapped to args


Set environnement variable : using env and variable

env
 - name: APP_COLOR
   value: red
 - name: APP_ENV
   value: prod

Set environnement variable : using configmap (see configmap and secret)
envFrom: 
 - configMapRef: 
   name: the-name-of-configmap1
 - configMapRef: 
   name: the-name-of-configmap2

Set environnement variable : using secret
envFrom
  - secretRef: 
    name: secretKeyRef1
  - secretRef: 
    name: secretKeyRef2

You can use a configmap to define one variable using : 
env
 - name: APP_COLOR
   valueFrom:
    configMapKeyRef:
     name: ref-to-config-name
     key: APP_COLOR

You can use a volume definition too