#  IAM for ServiceAccount

In the context of Amazon Elastic Kubernetes Service (EKS), an IAM OIDC provider is used to authenticate and authorise the AWS Load Balancer Controller (ALB) to manage AWS application load balancers. The AWS Load Balancer Controller uses the IAM OIDC provider to obtain an AWS identity token that is used to authenticate with the Kubernetes API Server and manage AWS Application Load Balancers.

To create an IAM OIDC provider on AWS for your EKS cluster, follow these steps:

1. Install the AWS CLI on your system.

2. Open a terminal or command line and make sure you are logged in to your AWS account.

3. Run the following AWS CLI command to create the IAM OIDC provider:

```
aws eks describe-cluster --name <your-cluster-name> --region <aws-region> --query "cluster.identity.oidc.issuer" --output text
```

This command returns the value of your cluster's OIDC issuer identifier. Save it in an environment variable for use in the next step.

4. Run the following AWS CLI command to create the IAM OIDC provider:

```
aws iam create-open-id-connect-provider --url https://oidc.eks.<aws-region>.amazonaws.com/id/<your-cluster-identifier> --thumbprint-list <your-cluster-thumbprint> --client-id-list sts.amazonaws.com --region <aws-region>
```

This command creates the IAM OIDC provider and associates it with your EKS cluster.

Replace ``<your-cluster-identifier>``, ``<your-cluster-thumbprint>`` and ``<aws-region>`` with the corresponding values for your cluster.

``<your-cluster-identifier>`` is the identifier of your EKS cluster.

`` <aws-region>`` is the AWS region in which your cluster is located.

``<your-cluster-thumbprint>`` is the certificate thumbprint value of your cluster. 

You can get this value by running the following command:

```
aws eks describe-cluster --name <your-cluster-name> --query "cluster.certificateAuthority.data" --output text | openssl base64 -d -A | openssl x509 -in /dev/stdin -noout -fingerprint | tr -d ':' | awk '{print $2}'
```

You can do all of the above to configure the IAM OIDC provider in your cluster using a single [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) command:

```
eksctl utils associate-iam-oidc-provider \
    --region <aws-region> \
    --cluster <your-cluster-name> \
    --approve
```


To verify whether the IAM OIDC provider has been created in AWS, you can run the following AWS CLI command:

```
aws iam list-open-id-connect-providers
```

Download IAM policy for the AWS Load Balancer Controller
```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json
```

Create an IAM policy called AWSLoadBalancerControllerIAMPolicy

```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
```

You can create an IAM role and a ServiceAccount for the load balancer driver, use the ARN from the previous step:

```
eksctl create iamserviceaccount \
--cluster=<cluster-name> \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::<AWS_ACCOUNT_ID>:policy/AWSLoadBalancerControllerIAMPolicy \
--approve
```


*[Home documents](../README.md)*