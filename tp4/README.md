# TP4 - WordPress Production avec Autoscaling

## Objectif
Deployer WordPress en production sur Kubernetes avec :
- MySQL persistant (StatefulSet + PVC 10Gi)
- WordPress persistant (Deployment + PVC 5Gi)
- Secrets geres par Kustomization
- Autoscaling avec HPA
- Acces local via Ingress (HTTP only)

## Composants

| Composant | Type | Image | Replicas | Persistance |
|-----------|------|-------|----------|-------------|
| WordPress | Deployment | bitnami/wordpress (latest) | 1 (HPA 2-5) | Oui (PVC 5Gi) |
| MySQL | StatefulSet | mysql:8.0 | 1 | Oui (PVC 10Gi) |

## Structure des fichiers

```
tp4/
├── README.md           # Documentation complete
├── kustomization.yaml  # Orchestration + secretGenerator
├── database.yaml       # MySQL Service
├── app.yaml            # WordPress
├── autoscaling.yaml    # HPA
├── ingress.yaml        # Ingress
└── Makefile            # Commandes Makefile

```

## Deploiement

### Avec Makefile
```bash
make deploy
make url
make clean
```

### Manuellement
```bash
kubectl apply -k tp4/
```

### 2. Verifier les secrets generes
```bash
kubectl get secrets
```

### 3. Attendre que MySQL soit pret
```bash
kubectl get statefulset mysql -w
```

### 4. Verifier les pods WordPress
```bash
kubectl get pods -l app=wordpress
```

### 5. Verifier les PVC
```bash
kubectl get pvc
```

### 6. Verifier le HPA
```bash
kubectl get hpa
```

### 7. Verifier l'Ingress
```bash
kubectl get ingress
```

### 8. Acceder a WordPress

```bash
minikube service wordpress --url
```

### 9. WordPress
- Credentials par defaut : **admin / bateau** (definis dans les secrets)

## Secrets geres par Kustomization

Le fichier `kustomization.yaml` genere automatiquement les Secrets Kubernetes :

```yaml
secretGenerator:
  - name: mysql-secrets
    literals:
      - MYSQL_ROOT_PASSWORD=bateau
      - MYSQL_DATABASE=wordpress_db
      - MYSQL_USER=wordpress_user
      - MYSQL_PASSWORD=bateau
  - name: wordpress-secrets
    literals:
      - WORDPRESS_USERNAME=admin
      - WORDPRESS_PASSWORD=bateau
      - WORDPRESS_EMAIL=wordpress@email.com
```

## Nettoyage

### Supprimer tous les composants
```bash
kubectl delete -k tp4/
```

### Supprimer les PVC (donnees)
```bash
kubectl delete pvc mysql-storage-mysql-0 wordpress-storage
```

### Verifier que tout est supprime
```bash
kubectl get all
kubectl get pvc
kubectl get secrets | grep -E "mysql|wordpress"
```
