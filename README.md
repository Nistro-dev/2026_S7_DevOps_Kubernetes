# 2026_S7_DevOps_Kubernetes

TPs Kubernetes - ESIEA S7 DevOps 2026

## Structure du repo

```
.
├── tp2/                    # TP2 - Monster App
│   └── monster_app_k8s/
├── tp3/                    # TP3 - Autoscaling
│   ├── deployment.yml
│   └── infinite-calls.yml
├── tp4/                    # TP4 - WordPress
│   ├── kustomization.yaml
│   ├── database.yaml
│   ├── app.yaml
│   └── autoscaling.yaml
└── ...
```

## TPs disponibles

| TP | Thème | Description |
|----|-------|-------------|
| [TP2](tp2/) | Déploiement complet | Déployer une application multi-conteneurs avec Services, Kustomization et Ingress |
| [TP3](tp3/) | Autoscaling (HPA) | Mettre en place l'autoscaling horizontal basé sur les métriques CPU |
| [TP4](tp4/) | WordPress Production | WordPress + MySQL avec Kustomization, Secrets, PVC et HPA (2-5 replicas) |

## Prérequis

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://docs.docker.com/get-docker/)