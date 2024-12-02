# Kubernetes scripts

This repository contains some useful scripts for operating Kubernetes.

- `create-kubeconfig`: Create a kubeconfig to access the apiserver with the specified serviceaccount token and outputs it to stdout.
- `wait-until-pods-ready`: Wait for all pods to be ready in the current namespace.
- `force-update-deployment`: Recreate the pods managed by the specified deployment.
- `apiversion2resources`: Convert API versions read from STDIN to resources in the form of `apiversion.resource`.

## How to use

**create-kubeconfig**

Explicitly create a long-lived service-account-token (if you know what you're doing!)

```bash
SA=my-sa
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: $SA-token
  annotations:
    kubernetes.io/service-account.name: $SA
type: kubernetes.io/service-account-token
EOF
```

Note that kubernetes populates the token for you.

Then call the script: `./create-kubeconfig my-sa`

Note: This version of the script requires a secret instead of a serviceaccount as input, because the secrets can no 
longer be found using the service account:

> If you view the ServiceAccount using:
> kubectl get serviceaccount build-robot -o yaml
> You can't see the build-robot-secret Secret in the ServiceAccount API objects .secrets field because that field is only populated with auto-generated Secrets.
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#manually-create-a-long-lived-api-token-for-a-serviceaccount

**apiversion2resources**

```
# Convert the supported API versions on the server
$ kubectl api-versions | apiversion2resources
```

## License

These scripts are released under the MIT License.
