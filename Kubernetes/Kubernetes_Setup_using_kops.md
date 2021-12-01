# Setup Kubernetes (K8s) Cluster on AWS


1. Create Ubuntu EC2 instance
1. install AWSCLI
   ```sh
    curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
    sudo apt update
    sudo apt install unzip python
    unzip awscli-bundle.zip
    #sudo apt-get install unzip - if you dont have unzip in your system
    ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
    
    If you need to upgrade python use below commands:
    
      # Remove python2
      sudo apt purge -y python2.7-minimal

      # You already have Python3 but 
      # don't care about the version 
      sudo ln -s /usr/bin/python3 /usr/bin/python

      # Same for pip
      sudo apt install -y python3-pip
      sudo ln -s /usr/bin/pip3 /usr/bin/pip

      # Confirm the new version of Python: 3
      python --version
    ```
    
      

1. Install kubectl on ubuntu instance
   ```sh
   curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
   ```

1. Install kops on ubuntu instance
   ```sh
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
    ```
1. Create an IAM user/role  with Route53, EC2, IAM and S3 full access

1. Attach IAM role to ubuntu instance
   ```sh
   # Note: If you create IAM user with programmatic access then provide Access keys. Otherwise region information is enough
   aws configure
   
   AWS Access Key ID [None]:        
   AWS Secret Access Key [None]: 
   Default region name [None]: us-east-2
   Default output format [None]:
   
    ```

1. Create a Route53 private hosted zone (you can create Public hosted zone if you have a domain)
   ```sh
   Routeh53 --> hosted zones --> created hosted zone  
   Domain Name: valaxy.net
   Type: Private hosted zone for Amzon VPC
   ```

1. create an S3 bucket
   ```sh
    aws s3 mb s3://bothwell.k8s.valaxy.net
   ```
1. Expose environment variable:
   ```sh
    export KOPS_STATE_STORE=s3://bothwell.k8s.valaxy.net
   ```

1. Create sshkeys before creating cluster
   ```sh
    ssh-keygen
    
   
   ```

1. Create kubernetes cluster definitions on S3 bucket
   ```sh
   kops create cluster --cloud=aws --zones=ap-south-1b --name=bothwell.k8s.valaxy.net --dns-zone=valaxy.net --dns private 
    ```

1. If you wish to update the cluster worker node sizes use below command 
   ```sh 
   kops edit ig --name=CHANGE_TO_CLUSTER_NAME nodes
   ```

1. Create kubernetes cluser
    ```sh
    kops update cluster bothwell.k8s.valaxy.net --yes
    ```

1. Validate your cluster
     ```sh
      kops validate cluster
    ```

1. To list nodes
   ```sh
   kubectl get nodes
   ```

1. To delete cluster
    ```sh
     kops delete cluster demo.k8s.valaxy.net --yes
    ```
   
#### Deploying Nginx pods on Kubernetes
1. Deploying Nginx Container
    ```sh
    kubectl create deploy sample-nginx --image=nginx --replicas=2 --port=80
    # kubectl deploy simple-devops-project --image=yankils/simple-devops-image --replicas=2 --port=8080
    kubectl get all
    kubectl get pod
   ```

1. Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them.
   ```sh
   kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
   # kubectl expose deployment simple-devops-project --port=8080 --type=LoadBalancer
   kubectl get services -o wide
   ```
