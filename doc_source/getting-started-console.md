# Getting started with Amazon ECR using the AWS Management Console<a name="getting-started-console"></a>

* create an Amazon ECR's repository -- via -- Amazon ECR console
* steps
  * open the Amazon ECR console
  * choose 
    * **Get Started**
    * **Private** | **Visibility settings**
  * specify a name for the repository
  * choose
    * tag mutability 
    * TODO:

1. For **Scan on push**, choose the image scanning setting for the repository\. Repositories configured to scan on push will start an image scan whenever an image is pushed, otherwise image scans need to be started manually\.
**Important**  
Configuring image scanning at the repository level has been deprecated in favor of configuring it at the registry level\. For more information, see [Image scanning](image-scanning.md)\.

1. For **KMS encryption**, choose whether to enable server\-side encryption using AWS KMS keys stored in the AWS Key Management Service service\. For more information about this feature, see [Encryption at rest](encryption-at-rest.md)\.

1. Choose **Create repository**\.

**Build, tag, and push a Docker image**

In this section of the wizard, you use the Docker CLI to tag an existing local image \(that you have built from a Dockerfile or pulled from another registry, such as Docker Hub\) and then push the tagged image to your Amazon ECR registry\. For more detailed steps on using the Docker CLI, see [Using Amazon ECR with the AWS CLI](getting-started-cli.md)\.

1. Select the repository you created and choose **View push commands** to view the steps to push an image to your new repository\.

1. Run the login command that authenticates your Docker client to your registry by using the command from the console in a terminal window\. This command provides an authorization token that is valid for 12 hours\.

1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository\. Using the docker build command from the console in a terminal window\. Make sure that you are in the same directory as your Dockerfile\.

1. Tag the image with your Amazon ECR registry URI and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step\. If you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

1. Push the newly tagged image to your repository by using the docker push command in a terminal window\.

1. Choose **Close**\.