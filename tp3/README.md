# TP3 - Autoscaling Kubernetes (HPA)

## Objectif
Mettre en place l'autoscaling horizontal (Horizontal Pod Autoscaler) sur un microservice pour gérer automatiquement la charge.

## Prérequis
- Minikube en cours d'exécution
- Metrics Server activé dans Minikube

## Architecture

| Service | Image | Port | Replicas | CPU Request | CPU Limit |
|---------|-------|------|----------|-------------|-----------|
| microsvc | prateeksingh1590/microsvc:1.1 | 8080 | 1-4 (auto) | 125m | 250m |
| infinite-calls | busybox | - | 1 | - | - |

## Structure des fichiers

```
tp3/
├── README.md
├── deployment.yml          # Déploiement du microservice avec ressources
└── infinite-calls.yml      # Générateur de charge infinie
```

## Déploiement

### 1. Activer le Metrics Server
```bash
minikube addons enable metrics-server
```

### 2. Déployer le microservice
```bash
kubectl apply -f tp3/deployment.yml
```

### 3. Créer l'Horizontal Pod Autoscaler
```bash
kubectl autoscale deployment microsvc --cpu-percent=1 --min=1 --max=4
```

### 4. Vérifier le HPA
```bash
kubectl get hpa
kubectl describe hpa microsvc
```

### 5. Démarrer le générateur de charge
```bash
kubectl apply -f tp3/infinite-calls.yml
```

### 6. Observer l'autoscaling
```bash
# Voir les métriques des pods
kubectl top pods

# Voir le status du HPA en temps réel
kubectl get hpa -w

# Voir les pods se créer
kubectl get pods -l run=microsvc -w
```

## Commandes utiles

### Démarrer le générateur de charge
```bash
kubectl apply -f tp3/infinite-calls.yml
```

### Arrêter le générateur de charge
```bash
kubectl delete deployment infinite-calls
```

### Vérifier les processus dans le conteneur
```bash
kubectl exec -it <infinite-calls-pod-name> -- sh -c "ps aux"
```

### Voir les logs du générateur
```bash
kubectl logs <infinite-calls-pod-name> --tail=20
```

### Supprimer le HPA
```bash
kubectl delete hpa microsvc
```

### Supprimer tous les déploiements
```bash
kubectl delete -f tp3/
```

## Résultats attendus

1. **Sans charge** : 1 replica de microsvc
2. **Avec charge infinie** : Le HPA scale automatiquement jusqu'à 4 replicas
3. **Après arrêt de la charge** : Le HPA scale down progressivement vers 1 replica (après quelques minutes)

## Observations

- Le HPA surveille l'utilisation CPU par rapport à la `request` (125m)
- Avec un seuil de 1%, le moindre trafic déclenche le scaling
- Le scale-up est rapide (quelques secondes)
- Le scale-down est plus lent (environ 5 minutes) pour éviter le flapping

## Troubleshooting

### HPA montre `<unknown>` pour les métriques
```bash
# Vérifier que le Metrics Server est actif
kubectl get pods -n kube-system | grep metrics-server

# Réactiver si nécessaire
minikube addons disable metrics-server
minikube addons enable metrics-server
```

### Les pods ne scalent pas
```bash
# Vérifier les events du HPA
kubectl describe hpa microsvc

# Vérifier les métriques
kubectl top pods
```
