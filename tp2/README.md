# TP2 - Déploiement complet d'une application

Déploiement de la stack **Monster App** sur Kubernetes avec Minikube.

## Architecture

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│  monstericon    │ ───▶ │   dnmonster     │ ───▶ │     redis       │
│   (Frontend)    │      │  (Générateur)   │      │    (Cache)      │
│  Port: 9090     │      │  Port: 8080     │      │  Port: 6379     │
│  LoadBalancer   │      │   ClusterIP     │      │   ClusterIP     │
└─────────────────┘      └─────────────────┘      └─────────────────┘
```

## Composants

| Service | Image | Port | Replicas | Type |
|---------|-------|------|----------|------|
| monstericon | tecpi/monster_icon:0.1 | 9090 | 5 | LoadBalancer |
| dnmonster | amouat/dnmonster:1.0 | 8080 | 5 | ClusterIP |
| redis | redis:latest | 6379 | 1 | ClusterIP |

## Structure des fichiers

```
monster_app_k8s/
└── monster_stack/
    ├── monstericon.yaml      # Service + Deployment frontend
    ├── dnmonster.yaml        # Service + Deployment générateur
    ├── redis.yaml            # Service + Deployment cache
    ├── monster-ingress.yaml  # Configuration Ingress
    └── kustomization.yaml    # Regroupe toutes les ressources
```

## Déploiement

### 1. Activer l'addon Ingress

```bash
minikube addons enable ingress
```

### 2. Appliquer la kustomization

```bash
kubectl apply -k monster_app_k8s/monster_stack/
```

### 3. Vérifier le déploiement

```bash
# Voir tous les objets créés
kubectl get all -l app=monsterstack

# Voir les services
kubectl get services

# Voir les pods
kubectl get pods --watch
```

### 4. Accéder à l'application

```bash
minikube service monstericon
```

## Nettoyage

```bash
kubectl delete -k monster_app_k8s/monster_stack/
```