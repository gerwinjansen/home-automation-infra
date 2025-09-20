Install ArgoCD
    helm install --create-namespace --namespace argocd argocd .\argocd\ -f .\argocd\values.yaml
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | openssl base64 -d
    kubectl port-forward -n argocd svc/argocd-server 8443:443
    https://localhost:8443

Create ca-key-pair
    openssl ec -in jansen-root-ca.key -out jansen-root-ca-plain.key
    kubectl create namespace cert-manager
    kubectl create secret tls -n cert-manager ca-key-pair --cert=jansen-root-ca.cer --key=jansen-root-ca-plain.key

Bootstrap
    kubectl apply -n argocd -f infra-apps/infra-project.yaml
    kubectl apply -n argocd -f infra-apps/infra-app-of-apps.yaml
    https://localhost