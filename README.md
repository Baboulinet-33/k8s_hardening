# k8s Hardening

## Network Policies

- deny-default.yml: Refuse les connexions entrantes par défaut
- allow-same-namespace.yml: Accepte les connexions du même ns
- allow-from-ingress.yml: Accepte les connexion depuis le namespace contenant l'ingress controller

## Namespace

Labels à appliquer au namespace utilisateurs:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  labels:
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/enforce-version: latest
    pod-security.kubernetes.io/warn: restricted
    pod-security.kubernetes.io/warn-version: latest
  name: mon-namespace
```

Cela permet d'intégrer des contrôles poussées au niveau de la sécurité:

- Volume Types (hostPath interdit, ...)
- Privilege Escalation
- Running as Non-root
- Running as Non-root user
- Seccomp
- Capabilities

A ajouter via kyverno pour avoir un filesystem en RO:

```yaml
securityContext:
    readOnlyRootFilesystem : true
```

## Service Account

Check si un service account a la propriété `automountServiceAccountToken` (ne monte pas le token lié au service account dans le pod):

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: mon-sa
  namespace: mon-namespace
automountServiceAccountToken: false
```
