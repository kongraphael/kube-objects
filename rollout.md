CREATE : kubectl create -f deploymentdefinition.yaml
GET : kubectl get deployment
UPDATE : kubectl apply -f deploymentdefinition.yaml
         kubectl set image deployment/mydeployment imageversion=imagenewversion

STATUS : kubectl rollout status deployment/mydeployment
         kubectl rollout history deployment/mydeployment
ROLLBACK : kubectl rollout undo deployment/mydeployment