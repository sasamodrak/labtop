# LaBTop
Deploy in a few minutes a local kubernetes cluster lab using terraform with cilium, ingress, ArgoCD

# requirements

- kind
- docker
- terraform

see all tools https://github.com/martijnvdp/labtop/tree/dependency#client-tools

Important: On windows with WSL you need to rebuild a WSL kernel for cilium:
more info https://github.com/martijnvdp/labtop#wsl-windows-cilium-enterprise

# usage example

```hcl
module "eks" {
  source = "git::https://github.com/martijnvdp/labtop.git?ref=v0.0.6"

  kindCluster = {
    name    = "labtop"
    version = "v1.21.14"

    config = {
      controlNodes = 1
      workerNodes  = 2
    }
  }
}
```
`terraform init` & `terraform plan` & `terraform apply`

## tools after install

- ArgoCD: http://argo-cd.127.0.0.1.nip.io
- Game 2048: http://game-2048.127.0.0.1.nip.io
- Hubble UI: http://hubble-ui.127.0.0.1.nip.io/
- LaBTop info: http://labtop-info.127.0.0.1.nip.io/ 

## argoCD
Get Base64 encoded default admin password:
http://labtop-info.127.0.0.1.nip.io/

## WSL Windows Cilium enterprise
to use cilium on windows icw WSL you need to rebuild a custom WSL kernel 

a script for this can be found here:  
`https://raw.githubusercontent.com/martijnvdp/bash-code-snippets/main/wsl2/build-wsl2-kernel.sh`

to download and run the script, open a wsl terminal and copy this line:
```
curl https://raw.githubusercontent.com/martijnvdp/bash-code-snippets/main/wsl2/build-wsl2-kernel.sh -o ./build-wsl-kernel.sh && chmod +x ./build-wsl-kernel.sh && ./build-wsl-kernel.sh
```

## Client tools

- terraform: https://developer.hashicorp.com/terraform/downloads
- helm: https://helm.sh/docs/intro/install/
- kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
- docker desktop: https://www.docker.com/products/docker-desktop/

<!--- BEGIN_TF_DOCS --->
## Requirements

| Name | Version |
|------|---------|
| helm | >= 2.2.0 |
| kind | 0.0.15 |

## Providers

| Name | Version |
|------|---------|
| helm | >= 2.2.0 |
| kind | 0.0.15 |
| null | n/a |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| argoCD | ArgoCD deployment settings | <pre>object({<br>    chart      = optional(string, "argo-cd")<br>    name       = optional(string, "argo-cd")<br>    namespace  = optional(string, "argo-cd")<br>    repository = optional(string, "https://argoproj.github.io/argo-helm")<br>    timeout    = optional(number, 600)<br>    version    = optional(string, "v5.7.0") # higher version requires k8 1.22+<br>  })</pre> | `{}` | no |
| argoCDApps | ArgoCD application(sets) and projects | <pre>object({<br>    chart      = optional(string, "argocd-apps")<br>    name       = optional(string, "argocd-apps")<br>    namespace  = optional(string, "argo-cd")<br>    repository = optional(string, "https://argoproj.github.io/argo-helm")<br>    version    = optional(string, "v0.0.3")<br>    deploy = optional(object({<br>      game2048 = optional(bool, true)<br>    }), {})<br>  })</pre> | `{}` | no |
| cilium | Cilium settings | <pre>object({<br>    chart      = optional(string, "cilium")<br>    name       = optional(string, "cilium")<br>    namespace  = optional(string, "kube-system")<br>    repository = optional(string, "https://helm.cilium.io/")<br>    version    = optional(string, "v1.12.4")<br>  })</pre> | `{}` | no |
| ingressController | Ingress controller settings | <pre>object({<br>    chart      = optional(string, "ingress-nginx")<br>    name       = optional(string, "ingress-nginx")<br>    namespace  = optional(string, "ingress-nginx")<br>    repository = optional(string, "https://kubernetes.github.io/ingress-nginx")<br>    values     = optional(string, "")<br>    version    = optional(string, "v4.4.0")<br>  })</pre> | `{}` | no |
| kindCluster | Cluster settings | <pre>object({<br>    name         = optional(string, "labtop")<br>    version      = optional(string, "v1.22.15")<br>    waitForReady = optional(bool, false)<br>    config = optional(object({<br>      controlNodes      = optional(number, 1)<br>      disableDefaultCNI = optional(bool, true)<br>      ingress           = optional(bool, true)<br>      workerNodes       = optional(number, 0)<br>    }), {})<br>  })</pre> | `{}` | no |

## Outputs

No output.

<!--- END_TF_DOCS --->
