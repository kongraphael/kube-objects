Pour pouvoir organiser le placement des pods sur les nodes il existe plusieurs méthodes ; il faut cependant ajouter des labels aux nodes ;

1/ Label
C'est un systeme de clé/valeur. On peut mettre ce que l'on veut :
zone:dmz
zone:bb
env:prod
env:preprod
env:dev
cpu:high
cpu:middle
cpu:low

https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/

C'est facilement configurable dans Rancher

2/ Taint et Toleration
Taint sont placés sur les nodes
Toleration sont placés sur les pods

Cette option empechera un pod d'etre schedulé sur un node "incompatible" ;  mais il n'empechera pas le pod d'etre schedulé sur un autre node, sans taint.

L'interet est limité, mais on peut s'assurer par ex, qu'un pod ne sera jamais lancé sur des node du backbone, ou de la DMZ, via cette méthode.

Remarque : le master node a une "taint"  noSchedule, ce qui empeche tout pod d'etre lancé dessus. Les taint apparaissent dans l'interface de rancher. Ce sont des petits textes jaunes, je crois, au niveau des nodes.

https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/

3/ Node Selector

Cette option est la plus simple pour permettre à un node d'etre schedulé sur un node en particulier.
Il suffira de placer un label sur les nodes, et de spécifier ce label sur le pod / deploiement.

Ex : zone=dmz pour les nodes en DMZ

Puis dans la defintion du pod, il faudra utiliser l'attribut nodeSelector
nodeSelector:
    zone: dmz

Remarque : il n'est pas possible de faire des combinaisons complexes avec nodeSelector (ex : zone: dmz ET disk: ssd)

https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector

4/ Node Affinity (NodeSelector ++)

La config yaml est plus complexe pour ce cas ; elle permet des choses plus avancées, comme pour l'ex du dessus (zone: dmz ET disk: ssd), ou encore disk différent de ssd

Je ne sais pas si c'est utilisable dans Rancher

https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity

5/ Mixe

Dans certains, on pourra mixer les taints et toleration avec les nodeSelector.