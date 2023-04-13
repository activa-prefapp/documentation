# IAM ExternalDNS

The installation of External DNS on your EKS cluster requires upfront configuration of the policies and roles needed for permission delegation.

Follow the steps below prior to the FluxCD deployment of the OAM component that will perform the installation.

Download the policy with the command:

```
curl -o route53-policy <link>
```

You must replace ``<<Hosted zone ID>`` with the ID of the Route53 hosted zone for which you are creating the policy.

Create the policy by running the following command in the terminal:

```
aws iam create-policy --policy-name route53-policy --policy-document file://route53-policy.json
```


Download the role with the command:

```
curl -o eks-trust-relationship <link>
```

You must replace ``<ACCOUNT_NUMBER>``, ``<AWS_REGION>`` and ``<OpenID_Connect_provider>`` with the appropriate values for your environment.

Create the role by running the following command in the terminal:

```
aws iam create-role --role-name eks-route53-role --assume-role-policy-document file://eks-trust-relationship.json
```

Add the previously created policy to the role by running the following command in the terminal:

```
aws iam attach-role-policy --policy-arn arn:aws:iam::<ACCOUNT_NUMBER>:policy/route53-policy --role-name eks-route53-role
```

Remember to replace ``<ACCOUNT_NUMBER>`` with your AWS account ID.

Now you can deploy external-DNS on your eks cluster via the oam-component of the [oam-applications repository](https://github.com/activa-prefapp/oam-applications).



*[Home documents](../README.md)*