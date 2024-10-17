# Private registry authentication<a name="registry_auth"></a>

* goal
  * registry authentication methods -- next subsections --
* ways to manage private repositories
  * AWS Management Console
  * AWS CLI
  * AWS SDKs
  * Amazon ECR API 
* standard AWS authentication methods

## Using the Amazon ECR credential helper<a name="registry-auth-credential-helper"></a>

* MFA NOT supported
* allows
  * making it easier | pushing & pulling images to Amazon ECR
    * store Docker credentials
    * use Docker credentials 
* [Amazon ECR Docker Credential Helper](https://github.com/awslabs/amazon-ecr-credential-helper)
  * how to 
    * install
    * configure

## Using an authorization token<a name="registry-auth-token"></a>

* authorization token
  * 's permission scope matches that of the IAM principal used to retrieve the authentication token
  * lifetime 
    * 12 hours
  * uses
    * access any Amazon ECR registry / your IAM principal has access to
  * ways to get it
    * [GetAuthorizationToken](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API operation
      * retrieve a base64\-encoded authorization token / contains
        * username `AWS`
        * encoded password
    * `get-login-password` AWS CLI
      * retrieve and decoding the authorization token

### To authenticate Docker to an Amazon ECR private registry with the CLI<a name="get-login-password"></a>

* options / -- pass to -- `docker login`
  * `AWS` -- for the -- username
  * Amazon ECR registry URI
* [get-login-password](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login-password.html) | AWS CLI
  ```
  aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
  ```
* [Get\-ECRLoginCommand](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRLoginCommand.html) | AWS Tools for Windows PowerShell

  ```
  (Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
  ```

## Using HTTP API authentication<a name="registry_auth_http"></a>

* [Docker Registry HTTP API](https://docs.docker.com/registry/spec/api/)
  * -- supported by -- Amazon ECR
  * requirements
    * authorization token / EVERY HTTP request
      * Reason: 🧠Amazon ECR is a private registry 🧠 
      * _Example1:_ `GetAuthorizationToken`
      * _Example2:_

       ```
       # retrieve the token & set to an environment variable
       TOKEN=$(aws ecr get-authorization-token --output text --query 'authorizationData[].authorizationToken')
      
       # use the token to make HTTP call to list the image tags | Amazon ECR repository
       curl -i -H "Authorization: Basic $TOKEN" https://aws_account_id.dkr.ecr.region.amazonaws.com/v2/amazonlinux/tags/list
       ```

      The output is as follows:

       ```
       HTTP/1.1 200 OK
       Content-Type: text/plain; charset=utf-8
       Date: Thu, 04 Jan 2018 16:06:59 GMT
       Docker-Distribution-Api-Version: registry/2.0
       Content-Length: 50
       Connection: keep-alive
       
       {"name":"amazonlinux","tags":["2017.09","latest"]}
       ```