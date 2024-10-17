# Setting up with Amazon ECR<a name="get-set-up-for-amazon-ecr"></a>

* set up process for ECR == set up process for Amazon ECS or Amazon EKS
  * Reason: 🧠Amazon ECR == extension of both ECS + EKS 🧠
* recommendations
  * if you are going to use AWS CLI -> use a version / supports the latest Amazon ECR features
* goal
  * setup ECR
  * push a container image -- to -- Amazon ECR

## Sign up for AWS<a name="sign-up-for-aws"></a>

* When you sign up for AWS -> your AWS account -- is automatically signed up for -- ALL services
* steps
  * create an AWS account
    * [AWS Portal Billing sign up](https://portal.aws.amazon.com/billing/signup)
  * note your AWS account number

## Create an IAM user<a name="create-an-iam-user"></a>

* requirements
  * provide credentials | you access -- via -- 
    * AWS CLI 
      * create access keys / your AWS account
    * AWS IAM & AWS Console
      * 👁️recommended 👁
      * requirements
        * == create an IAM user / -- added to an -- IAM group / has administrative permissions
          * sign in to the [IAM console](https://console.aws.amazon.com/iam/) -- as the -- **Root user**
            * [service management tasks / require root user](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)
          * select the **AdministratorAccess** | policy list 
      * access -- via -- a special URL / credentials for the IAM user
        * ```
          https://your_aws_account_id.signin.aws.amazon.com/console/
          ```
            * _Example:_ if the AWS account number is `1234-5678-9012` -> `your_aws_account_id` = `123456789012`
        * [your AWS account ID -- can be replaced by an -- alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html)
