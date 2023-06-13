## Kubernetes secrets for GitOps

For your infrastructure deployment using GitOps via FluxCD in KubeVela you will need to configure some secrets for access to a private repository and/or a private image registry.

For the OAM GitOps application defined in oam-applications repository we have configured the secrets needed to access the private repository oam-gitops-sample and its image registry.

You may notice in the [original documentation](https://fluxcd.io/flux/components/source/gitrepositories/#basic-access-authentication), FluxCD expects a basic authentication secret. As specified in the section "[authentication with bearer token](https://fluxcd.io/flux/components/source/gitrepositories/#bearer-token-authentication)", in the note after the first paragraph, in this case the password field must store the github token with sufficient accesses and not the usual password.

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