# Studies on GitOps using ArgoCD

This suite uses ArgoCD to deploy observability applications from the private github repository. Features include application deployment and the ability to configure alerts via Telegram.

## Installation

To uses this suite, you need a local custer with minikube. If necessary, consult the documentation for these tools.
[minikube](https://minikube.sigs.k8s.io/docs/start/)

1. You first need create a namespace for ArgoCD.
```kubectl create namespace argocd```

2. Apply the installation file.
```kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml```

3. Change the service type to LoadBalancer.
```kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'```

4. Get the credentials to login in ArgoCD.
```kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo```

5. Get the URL to access ArgoCD.
```export ARGO_IP=$(minikube service -n argocd argocd-server --url | head -1) && echo $ARGO_IP```

6. Access the URL and put the user admin and your credential generated in step 4.

7. Edit ./apps/alertmanager/configmap.yaml with your telegram informations.

8. Edit config files in ./argocd and ./config with your git personal token.

9. Deploy app-of-apps.yaml to deploy the all apps in './apps'.
```kubectl apply -f ./argocd/app-of-apps.yaml```
