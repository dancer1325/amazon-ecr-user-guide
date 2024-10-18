# Amazon Elastic Container Registry Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

* By default, IAM users and roles do NOT have permission to
  * create or modify Amazon ECR resources
  * can NOT perform tasks -- via the --
    * AWS Management Console
    * AWS CLI
    * AWS API 
* IAM administrator must
  * create IAM policies / grant users and roles permission -- to perform -- specific API operations | specified resources
    * [Creating IAM Identity-based Policies | JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) 
  * attach those policies -- to the -- corresponding IAM users or groups

**Topics**
+ [Policy Best Practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the Amazon ECR Console](#security_iam_id-based-policy-examples-console)
+ [Allow Users to View Their Own Permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Accessing One Amazon ECR Repository](#security_iam_id-based-policy-examples-access-one-bucket)

## Policy Best Practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

* [Security best practices | IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

## Using the Amazon ECR Console<a name="security_iam_id-based-policy-examples-console"></a>

* Amazon Elastic Container Registry console
  * if you want to access -> you must have a minimum set of permissions (_Example:_ list & view details -- about the -- Amazon ECR resources | your AWS account)
  * 👁️if you create an identity-based policy / more restrictive -- than the -- minimum required permissions -> console will NOT function for entities \(IAM users or roles\) / that policy 👁️
    * -> add the `AmazonEC2ContainerRegistryReadOnly` AWS managed policy | those entities
      * [Adding Permissions to a User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console)

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "ecr:GetAuthorizationToken",
                    "ecr:BatchCheckLayerAvailability",
                    "ecr:GetDownloadUrlForLayer",
                    "ecr:GetRepositoryPolicy",
                    "ecr:DescribeRepositories",
                    "ecr:ListImages",
                    "ecr:DescribeImages",
                    "ecr:BatchGetImage",
                    "ecr:GetLifecyclePolicy",
                    "ecr:GetLifecyclePolicyPreview",
                    "ecr:ListTagsForResource",
                    "ecr:DescribeImageScanFindings"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

  * 👁️if users are making calls ONLY to the AWS CLI or the AWS API -> 👁️
    * NOT need to allow minimum console permissions 
    * allow access ONLY the actions / match the API operation / you're trying to perform

## Allow Users to View Their Own Permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

* policy / IAM user -- can view the -- inline & managed policies / attached to their user identity | console or programmatically -- via -- AWS CLI or AWS API

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "ViewOwnUserInfo",
                "Effect": "Allow",
                "Action": [
                    "iam:GetUserPolicy",
                    "iam:ListGroupsForUser",
                    "iam:ListAttachedUserPolicies",
                    "iam:ListUserPolicies",
                    "iam:GetUser"
                ],
                "Resource": ["arn:aws:iam::*:user/${aws:username}"]
            },
            {
                "Sid": "NavigateInConsole",
                "Effect": "Allow",
                "Action": [
                    "iam:GetGroupPolicy",
                    "iam:GetPolicyVersion",
                    "iam:GetPolicy",
                    "iam:ListAttachedGroupPolicies",
                    "iam:ListGroupPolicies",
                    "iam:ListPolicyVersions",
                    "iam:ListPolicies",
                    "iam:ListUsers"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

## Accessing One Amazon ECR Repository<a name="security_iam_id-based-policy-examples-access-one-bucket"></a>

* policy / IAM user -- can -- 
  * access to one of your Amazon ECR repositories, `my-repo`
  * push, pull, and list images

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"ListImagesInRepository",
         "Effect":"Allow",
         "Action":[
            "ecr:ListImages"
         ],
         "Resource":"arn:aws:ecr:us-east-1:123456789012:repository/my-repo"
      },
      {
         "Sid":"GetAuthorizationToken",
         "Effect":"Allow",
         "Action":[
            "ecr:GetAuthorizationToken"
         ],
         "Resource":"*"
      },
      {
         "Sid":"ManageRepositoryContents",
         "Effect":"Allow",
         "Action":[
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:InitiateLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:CompleteLayerUpload",
                "ecr:PutImage"
         ],
         "Resource":"arn:aws:ecr:us-east-1:123456789012:repository/my-repo"
      }
   ]
}
```