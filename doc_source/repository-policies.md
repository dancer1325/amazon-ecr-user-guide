# Private repository policies<a name="repository-policies"></a>

* Amazon ECR, to control access to repositories -- uses -- 👁️resource-based permissions 👁️
  * resource\-based permissions let you specify
    * IAM users or roles / -- have access to a -- repository
    * actions / can perform | it
* by default, ONLY the AWS account / created the repository -- has access to a -- repository
  * policy document -- can allow -- additional permissions | your repository

**Topics**
+ [Repository policies vs IAM policies](#repository-policy-vs-iam-policy)
+ [Setting a private repository policy statement](set-repository-policy.md)
+ [Deleting a private repository policy statement](delete-repository-policy.md)
+ [Private repository policy examples](repository-policy-examples.md)

## Repository policies vs IAM policies<a name="repository-policy-vs-iam-policy"></a>

* Amazon ECR repository policies
  * == subset of IAM policies 
    * / are scoped for & specifically used for
      * -- controlling access to -- individual Amazon ECR repositories
    * uses
      * -- apply permissions for the -- entire Amazon ECR service
      * -- control access to -- specific resources
* Amazon ECR repository policies vs IAM policies
  * uses
    * determine the actions / specific IAM user or role -- may -- perform | repository
      * 👁️if one of repository policies or IAM policies -> action is denied👁️
    * TODO:

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

You can use either of these policy types to control access to your repositories, as shown in the following examples\.

This example shows an Amazon ECR repository policy, which allows for a specific IAM user to describe the repository and the images within the repository\.

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "ECR Repository Policy",
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::account-id:user/username"
    },
    "Action": [
       "ecr:DescribeImages",
       "ecr:DescribeRepositories"
    ]
  }]
}
```

This example shows an IAM policy that achieves the same goal as above, by scoping the policy to a repository \(specified by the full ARN of the repository\) using the resource parameter\. For more information about Amazon Resource Name \(ARN\) format, see [Resources](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies-resources)\.

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "ECR Repository Policy",
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::account-id:user/username"
    },
    "Action": [
      "ecr:DescribeImages",
      "ecr:DescribeRepositories"
    ],
    "Resource": [
      "arn:aws:ecr:region:account-id:repository/repository-name"
    ]
    }]
}
```