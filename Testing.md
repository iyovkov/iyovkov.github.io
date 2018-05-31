# Deploying CVA 

## Overview:

**CVA** stands for **C**lient **V**irtual **A**ppliance. The CVA is not finalized, so the necessary containers will be deployed manually for the moment. It will be owned and managed by Platform DXC and it contains the following containers.

- Raffia
- Ansible
- BigFix
- Nagios
- Logstash
- Universal Discovery.

For MSVMC we will use only Raffia, Ansible and BigFix.

## Deploy Docker CE on Ubunto

1. Create a new EC2 instance by using Ubunto 16.04 AMI. As soon as the machine is up and running, logon and continue with the next steps.

2. Setup the repository:
   - Update the **apt** package index

     ```
     $ sudo apt-get update
     ```

   - Install packages to allow apt to use a repository over HTTPS

     ```
     $ sudo apt-get install \
         apt-transport-https \
         ca-certificates \
         curl \
         software-properties-common
     ```

   - Add Docker’s official GPG key 

     ```
     $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     ```

     Verify that you now have the key with the fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint. 

     ```
     $ sudo apt-key fingerprint 0EBFCD88
     
     pub   4096R/0EBFCD88 2017-02-22
           Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
     uid                  Docker Release (CE deb) <docker@docker.com>
     sub   4096R/F273FCD8 2017-02-22
     ```

   - Use the following command to set up the **stable** repository. You always need the **stable** repository, even if you want to install builds from the **edge** or **test** repositories as well. To add the **edge** or **test** repository, add the word `edge` or `test` (or both) after the word `stable` in the commands below. 

     **Note**: The `lsb_release -cs` sub-command below returns the name of your Ubuntu distribution, such as `xenial`. Sometimes, in a distribution like Linux Mint, you might need to change `$(lsb_release -cs)` to your parent Ubuntu distribution. For example, if you are using `Linux Mint Rafaela`, you could use `trusty`. 

     ```
     $ sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
     ```

     **Note**: Starting with Docker 17.06, stable releases are also pushed to the **edge** and **test** repositories. 

3. Install Docker CE

   - Update the `apt` package index. 

     ```
     $ sudo apt-get update
     ```

   - Install the *latest version* of Docker CE, or go to the next step to install a specific version: 

     ```
     $ sudo apt-get install docker-ce
     ```

     **Note:** If you have multiple Docker repositories enabled, installing or updating without specifying a version in the `apt-get install` or `apt-get update` command always installs the highest possible version, which may not be appropriate for your stability needs. 

   - To install a *specific version* of Docker CE, list the available versions in the repo, then select and install: 

     - List the versions available in your repo: 

       ```
       $ apt-cache madison docker-ce
       
       docker-ce | 18.03.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
       ```

     - Install a specific version by its fully qualified package name, which is the package name (`docker-ce`) plus the version string (2nd column) up to the first hyphen, separated by a an equals sign (`=`), for example, `docker-ce=18.03.0.ce`. 

       ```
       $ sudo apt-get install docker-ce=<VERSION>
       ```

     - The Docker daemon starts automatically. 

     - Verify that Docker CE is installed correctly by running the `hello-world` image. 

       ```
       $ sudo docker run hello-world
       ```

     - This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits. 

       

     Docker CE is installed and running. The `docker` group is created but no users are added to it. You need to use `sudo` to run Docker commands. Continue to [Linux postinstall](https://docs.docker.com/install/linux/linux-postinstall/) to allow non-privileged users to run Docker commands and for other optional configuration steps. 

## Install NodeJS 

curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -  sudo apt-get install -y nodejs

## Deploying Raffia
