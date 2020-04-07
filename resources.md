# Resources mini required by default :
CPU : .5
MEM : 256Mo

Minimun CPU value: .1
.1 CPU can be said as 100m (100 milli)

By default the limit is 1vCPU and 512Mi

For the POD to pick up those defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.

See 
    apiVersion: v1
    kind: LimitRange
    metadata:
      name: mem-limit-range
    spec:
      limits:
      - default:
          memory: 512Mi
        defaultRequest:
          memory: 256Mi
        type: Container


Set for container !!!!

If memory used is above limit, kubernetes will kill the container

# Yaml
In spec section, add resources: request: memory: "1Gi" cpu: 1 for exemple

See pod-basic.yml