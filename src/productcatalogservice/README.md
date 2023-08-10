# productcatalogservice

Exécutez la commande suivante pour restaurer les dépendances

    dep ensure --vendor-only

## Dynamic catalog reloading / artificial delay

Ce service dispose d'une fonction de "rechargement dynamique du catalogue" qui est volontairement
volontairement mal implémentée. L'objectif de cette fonctionnalité est de  permettre de modifier le
`products.json` & volonté et que les modifications soient prises en compte sans avoir à redémarrer le service.


Cependant, cette fonctionnalité est boguée : le catalogue est en fait rechargé à chaque requête, introduisant un délai notable dans le frontend.
ce qui introduit un délai notable dans le frontend. Ce délai sera également
dans les outils de profilage: the `parseCatalog` prendra plus de 80 % du temps de l'unité centrale.


Vous pouvez déclencher cette fonction (et le délai) en envoyant un`USR1` et
le supprimer (si nécessaire) en envoyant un `USR2` signal:

```
# Trigger bug
kubectl exec \
    $(kubectl get pods -l app=productcatalogservice -o jsonpath='{.items[0].metadata.name}') \
    -c server -- kill -USR1 1
# Remove bug
kubectl exec \
    $(kubectl get pods -l app=productcatalogservice -o jsonpath='{.items[0].metadata.name}') \
    -c server -- kill -USR2 1
```

## Latency injection

Ce service dispose d'un `EXTRA_LATENCY` variable d'environnement. Ceci injectera une veille pour le [time.Duration](https://golang.org/pkg/time/#ParseDuration) à chaque appel à
au serveur.

Par exemple, utilisez `EXTRA_LATENCY="5.5s"` se met en veille pendant 5,5 secondes à chaque demande.
