# What is Amazon Elastic Container Registry (ECR)?<a name="what-is-ecr"></a>

* == AWS managed container image registry service /
  * secure,
  * scalable,
  * reliable
  * supports
    * private container image repositories / resource-based permissions -- via -- AWS IAM
      * -> can access
        * specified users or
        * Amazon EC2 instances
    * public container image repositories
      * [Amazon ECR Public](https://docs.aws.amazon.com/AmazonECR/latest/public/what-is-ecr.html)
  * -- via your -- preferred CLI, allowed
    * actions
      * push
      * pull
      * manage
    * images
      * Docker images
      * OCI images
      * OCI compatible artifacts
* [AWS Containers Team Roadmap](https://github.com/aws/containers-roadmap)

## Components of Amazon ECR<a name="ecr-components"></a>

* Amazon ECR private Registry
  * provided / EACH AWS account
  * allows
    * creating >=1 repositories
    * storing images in them
  * [more here](Registries.md)

* Authorization token  
  * uses
    * if your client wants to push & pull images -> MUST authenticate as an AWS user| Amazon ECR registries 
  * [more here](registry_auth.md)

* Repository  
  * contains your
    * Docker images,
    * OCI images,
    * OCI compatible artifacts
  * [more here](Repositories.md)

* Repository policy  
  * allows
    * controlling access to your
      * repositories
      * images
  * [more here](repository-policies.md)

* Container Image  
  * can be
    * -- push -- to your repositories
    * pull -- from your repositories
  * uses
    * locally 
    * [Using Amazon ECR images | Amazon ECS task definitions](ECR_on_ECS.md)
    * [Using Amazon ECR Images | Amazon EKS pod specifications](ECR_on_EKS.md)

## Features of Amazon ECR<a name="ecr-features"></a>

* Lifecycle policies
  * uses
    * help with managing the lifecycle of the images | your repositories
  * allows
    * defining rules / clean up unused images
    * testing rules | before applying them to -- your repository
  * [Lifecycle policies](LifecyclePolicies.md)
* Image scanning
  * uses
    * identifying software vulnerabilities | your container images
      * **scan on push**
        * can be configured / repository
        * == each new image pushed to the repository is scanned
  * [Image scanning](image-scanning.md)
* Cross\-Region and cross\-account replication
  * allows
    * having your images | you need them 
  * how to configure it?
    * | registry setting / per\-Region basis
  * [Private registry settings](registry-settings.md)\
* Pull through cache rules
  * allows
    * caching your private Amazon ECR registry's repositories | remote public registries  
      * Amazon ECR will periodically check the cached image is up to date
  * [Using pull through cache rules](pull-through-cache.md)\.

## How to get started with Amazon ECR<a name="ecr-get-started"></a>

* install
  * AWS CLI
  * Docker
* check
  * [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md)
  * [Using Amazon ECR with the AWS CLI](getting-started-cli.md)

## Pricing for Amazon ECR<a name="ecr-pricing"></a>

* pay for the
  * amount of data / stored | your repositories
  * data / transfer -- from your image pushes and pulls
* [Amazon ECR pricing](http://aws.amazon.com/ecr/pricing/)