# k8-argo1



```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get pods -n argocd
```

#### Exposer le port pour accéder à l'interface de management

```bash
# Remplacer 'Type: ClusterIP' par 'NodePort'
kubectl edit svc argocd-server -n argocd

# Get les ports:
kubectl get svc argocd-server -n argocd
```

#### Get 'admin' password
```bash
# Récupérer mdp de admin:
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d; echo
```

Ensuite, se connecter sur l'UI de ArgoCD.
Créer une application.

---


Voici les principaux paramètres que tu dois remplir lors de la création de l'application :

- Application Name : Le nom de ton application. Par exemple, my-webapp.
- Project : Laisse default ou choisis un projet si tu en as configuré un.
- Sync Policy : Sélectionne Automatic si tu veux que les changements dans le dépôt Git se synchronisent automatiquement avec Kubernetes sans intervention manuelle.



Sous la section Source, tu dois définir où se trouvent les manifests ou fichiers YAML de ton application.

- Repository URL : L'URL de ton dépôt Git où se trouvent les fichiers de configuration Kubernetes. 
- Revision : La branche du dépôt que tu veux suivre. Par exemple main ou master.
- Path : Le chemin relatif dans le dépôt où se trouvent les fichiers YAML (comme manifests/ si tes manifests sont dans un sous-dossier).

Sous la section Destination, tu définis sur quel cluster Kubernetes et dans quel namespace l'application doit être déployée.
- Cluster URL : Laisse https://kubernetes.default.svc si tu veux déployer sur le cluster local.
- Namespace : Spécifie le namespace où tu veux que l'application soit déployée. Si le namespace n'existe pas, ArgoCD peut le créer pour toi.

Dans la section Sync Policy, tu peux activer la synchronisation automatique :

- Automatic : Si activé, ArgoCD appliquera automatiquement les modifications détectées dans le dépôt Git au cluster Kubernetes.
- Prune : Si activé, ArgoCD supprimera automatiquement les ressources qui ne sont plus définies dans le dépôt Git.

