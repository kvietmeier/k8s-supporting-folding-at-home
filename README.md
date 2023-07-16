# Fork of - [k8s supporting folding@home](https://github.com/wind0r/k8s-supporting-folding-at-home/tree/master)

This repository contains a dockerfile, a helm chart and also a simple Kubernetes configuration to get folding@home running on your own clusters.

I am currently running this using a simple deployment.yaml on Azure AKS-EE Kubernetes clusters on Intel NUCs and a Lenovo T15 Running Windows 11 Pro/Ent. My goal is to start with the simple case and add complexity.

## ToDo

- [Deploy/manage through Azure GitOps](https://learn.microsoft.com/en-us/azure/aks/hybrid/aks-edge-howto-use-gitops)
- Deploy multiple pods to consume more resources
- Edit resources to max out a single pod
- Enable GPU - need MSFT to enable GPU passthrough for AKS-EE 

## Kubernetes

This version creates a deployment with resource limits and sets folding-powerlevel to "full" so beware. You can change user, team and powerlevel via environment variables but I hard coded it in the deployment.yaml file for now, feel free to Fold as my team.

```powershell
kubectl apply -f https://raw.githubusercontent.com/kvietmeier/k8s-supporting-folding-at-home/master/kubernetes/deployment.yaml
```

Deployment creates a LoadBalancer service and assigns an IP from the address pool -

``` yaml
# Working now
# Need to use full name for app selector
---
apiVersion: v1
kind: Service
metadata:
  name: folding
spec:
  type: LoadBalancer
  ports:
  - port: 7396
  selector:
    app.kubernetes.io/name: folding-at-home
```

``` powershell
KV C:\Users\ksvietme\repos\k8s-supporting-folding-at-home> kubectl get deployments -o wide
NAME              READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS        IMAGES                          SELECTOR
folding-at-home   1/1     1            1           17m   folding-at-home   wind0r/folding-at-home:latest   app.kubernetes.io/name=folding-at-home,app.kubernetes.io/version=latest

KV C:\Users\ksvietme\repos\k8s-supporting-folding-at-home> kubectl get svc
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
folding      LoadBalancer   10.104.17.249   192.168.1.210   7396:30725/TCP   9m3s
kubernetes   ClusterIP      10.96.0.1       <none>          443/TCP          22h

```

## Helm (untested/draft)

This deploys a daemonset via helm with folding-powerlevel set to medium. You can add resource limitation and change other settings via values files like with every other helm chart.

``` powershell
git clone https://github.com/wind0r/k8s-supporting-folding-at-home.git
helm install folding ./k8s-supporting-folding-at-home/helm
```

## Settings

- `USER`: user name, defaults to "Anonymous"
- `TEAM`: team ID, defaults to "0"
- `PASSKEY`: optional, get one here: https://apps.foldingathome.org/getpasskey
- `POWER`: "light", "medium", or "full"

## Other Forks

- [run as deployment in unprivileged contexts by @hjacobs](https://codeberg.org/hjacobs/folding-at-home-on-kubernetes/src/branch/master/README.md)
