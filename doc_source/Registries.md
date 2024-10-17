# Amazon ECR private registry<a name="Registries"></a>

* allows
  * hosting your container images
    * | architecture / highly  
      * available
      * scalable
    * / types of images are
      * Docker images & artifacts
      * OCI images  & artifacts
* 👁️exist a default private Amazon ECR registry / AWS account 👁️

## Private registry concepts<a name="registry_concepts"></a>

* `https://``aws_account_id.dkr.ecr.region.amazonaws.com`
  * URL for your default private registry
* about access & permissions
  * by default, your account -- has -- read & write access to the repositories | your private registry
  * IAM users -- require -- permissions to
    * make calls to the Amazon ECR APIs
    * push or pull images -- to & from -- your private repositories
  * built-in several managed policies / control user access at varying levels
    * [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)
  * ways to control it -- [more here](repository-policies.md) --
    * IAM user access policies
    * repository policies
* your Docker client -- must be authenticated to your -- private registry
  * -> you can use `docker push` and `docker pull`
  * [Private registry authentication](registry_auth.md)
* repositories | your private registry -- can be [replicated](replication.md) across -- 
  * Regions | your own private registry
  * separate accounts