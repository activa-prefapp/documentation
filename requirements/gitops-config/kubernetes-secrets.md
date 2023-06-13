## Kubernetes secrets for GitOps

Configure your secret for repository access following the example:

```
apiVersion: v1
kind: Secret
metadata:
  name: git-auth-secret
type: Opaque
data:
  username: <BASE64_username>
  password: <BASE64_token>

```
In order to use a private image registry you can configure your secret following the example below:

```
apiVersion: v1
kind: Secret
metadata:
  name: git-registry-secret
type: kubernetes.io/dockerconfigjson
data:
  .dockerconfigjson: <BASE64_encoded_credentials>
```
Where the unencrypted config.json can be:

```
{
    "auths": {
      "https://ghcr.io": {
        "username": "<username>",
        "password": "<GitHub-token>",
        "email": "<email>"
      }
    }
  }
```