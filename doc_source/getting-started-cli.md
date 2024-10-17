# Using Amazon ECR with the AWS CLI<a name="getting-started-cli"></a>

* goal
  * push a container image -- to a -- private Amazon ECR repository / -- via -- Docker CLI & AWS CLI
    * [alternative tools](http://aws.amazon.com/tools/)

## Prerequisites<a name="getting-started-cli-prereqs"></a>

* [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md)

### Install the AWS CLI<a name="cli-install"></a>

* vs AWS Console
  * recommended
* [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) 

### Install Docker<a name="cli-install-docker"></a>

* [Docker installation guide](https://docs.docker.com/engine/installation/#installation)
* development systems
  * local 
    * == your own laptop
  * Amazon EC2 
    * [launch an ** instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)
    * [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html)
    * install Docker here
      * _Example:_ Linux instance
      
        ```
        sudo yum update -y
        
        sudo amazon-linux-extras install docker
        
        sudo service docker start
        
        # Add the `ec2-user` -- to the -- `docker` group 
        sudo usermod -a -G docker ec2-user
        
        # close your current SSH terminal & open again OR reboot the instance
        # verify that `ec2-user` -- can run -- Docker commands / WITHOUT `sudo` 
        docker info        
        ```

## Step 1: Create a Docker image<a name="cli-create-image"></a>

* [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)

   ```
   touch Dockerfile
   ```
  * _Example:_

   ```
   FROM public.ecr.aws/docker/library/ubuntu:18.04
   
   # Install dependencies
   RUN apt-get update && \
    apt-get -y install apache2
   
   # Install apache and write hello world message
   RUN echo 'Hello World!' > /var/www/html/index.html
   
   # Configure apache
   RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
    echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
    echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \ 
    echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \ 
    chmod 755 /root/run_apache.sh
   
   EXPOSE 80
   
  # Starts the web server
   CMD /root/run_apache.sh
   ```
  * next steps  

   ```
   # `.`    or full path
   docker build -t hello-world .
  
   # verify the image was created
   docker images --filter reference=hello-world
  
   # run the new built image
   docker run -t -i -p 80:80 hello-world
  ```
  * check that "Hello World\!" is displayed | server
    * if you are using an EC2 instance -> use SAME address -- to connect, via SHH, to the -- instance
      * security group for your instance -- should allow -- inbound traffic | port 80
    * if you are running Docker locally -> point your browser | http://localhost/
    * if you are using docker\-machine | Windows or macOS computer -> find the IP address of the VirtualBox VM / hosts Docker
      ```
      docker-machine ip machine-name
      ``` 
           
  * `Ctrl + C`
    * stop the Docker container

## Step 2: Authenticate to your default registry<a name="cli-authenticate-registry"></a>

* authenticate -- to your -- 👁️default registry👁️
  * [ecr get-login-password](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login-password.html)
    
      ```
      # pipe to `docker login` -- via -- --password-stdin 
      aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
      ```
    * if you want to authenticate to multiple registries -> repeat the command / EACH registry

  * [Get\-ECRLoginCommand](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRLoginCommand.html) | Windows PowerShell

    ```
    # pipe to `docker login` -- via -- --password-stdin
    (Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
    ```

## Step 3: Create a repository<a name="cli-create-repository"></a>

* create a repository / hold the built image
  * _Example:_

    ```
    aws ecr create-repository \
        --repository-name hello-world \
        --image-scanning-configuration scanOnPush=true \
        --region region
    ```

## Step 4: Tag and Push an image to Amazon ECR<a name="cli-push-image"></a>

* docker CLI
  * allows
    * 👁️pushing the images 👁️
* prerequisites
  * docker v1.7+
  * Amazon ECR authorization token -- has been configured with -- `docker login`
  * Amazon ECR repository
    * exists
    * / user has access -- to push to -- it
* steps
  * list the images / stored locally -- to identify the -- image to tag and push

     ```
     docker images
     ```

     Output:

     ```
     REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
     hello-world         latest              e9ffedc8c286        4 minutes ago       241MB
     ```

  * tag the image / it's going to be pushed to your repository

     ```
     docker tag hello-world:latest aws_account_id.dkr.ecr.region.amazonaws.com/hello-world:latest
     ```

  * Push the image

     ```
     docker push aws_account_id.dkr.ecr.region.amazonaws.com/hello-world:latest
     ```

     Output:

     ```
     The push refers to a repository [aws_account_id.dkr.ecr.region.amazonaws.com/hello-world] (len: 1)
     e9ae3c220b23: Pushed
     a6785352b25c: Pushed
     0998bf8fb9e9: Pushed
     0a85502c06c9: Pushed
     latest: digest: sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b size: 6774
     ```

## Step 5: Pull an image from Amazon ECR<a name="cli-pull-image"></a>

* container image
  * -- can be pull it from -- OTHER locations
* prerequisites 
  * docker v1.7+
  * Amazon ECR authorization token -- has been configured with -- `docker login`
  * Amazon ECR repository
    * exists
    * / user has access -- to pull from -- it


    ```
    docker pull aws_account_id.dkr.ecr.region.amazonaws.com/hello-world:latest
    ```

    Output:
    
    ```
    latest: Pulling from hello-world
    0a85502c06c9: Pull complete
    0998bf8fb9e9: Pull complete
    a6785352b25c: Pull complete
    e9ae3c220b23: Pull complete
    Digest: sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b
    Status: Downloaded newer image for aws_account_id.dkr.region.amazonaws.com/hello-world:latest
    ```

## Step 6: Delete an image<a name="cli-delete-image"></a>

* 

    ```
    aws ecr batch-delete-image \
          --repository-name hello-world \
          --image-ids imageTag=latest \
          --region region
    ```

    Output:
    
    ```
    {
        "failures": [],
        "imageIds": [
            {
                "imageTag": "latest",
                "imageDigest": "sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b"
            }
        ]
    }
    ```

## Step 7: Delete a repository<a name="cli-delete-repository"></a>

* if you want to delete a repository / contains images -> use `--force`

    ```
    aws ecr delete-repository \
          --repository-name hello-world \
          --force \
          --region region
    ```