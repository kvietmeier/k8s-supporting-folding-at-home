# Fork of - [k8s supporting folding@home](https://github.com/wind0r/k8s-supporting-folding-at-home/tree/master)

This repository contains a dockerfile, a helm chart and also a simple Kubernetes configuration to get folding@home running on your own clusters.

I am currently running this on Azure AKS-EE Kubernre6es clusters on Intel NUCs.

## Kubernetes

This version creates a deployment with resource limits and sets folding-powerlevel to "light" so it should not take that many resources. You can change user, team and powerlevel via environment variables.

```powershell
kubectl apply -f https://raw.githubusercontent.com/kvietmeier/k8s-supporting-folding-at-home/master/kubernetes/daemonset.yaml
```

To assign a local IP:port -

``` powershell
kubectl expose deployment folding-at-home --type="LoadBalancer" --name=folding
```

## Helm (untested)

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
