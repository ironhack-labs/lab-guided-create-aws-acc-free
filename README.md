![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Guided exercise: How to Create AWS Account Free

This video provides a detailed and concise tutorial on how to create a free AWS account quickly. It includes step-by-step instructions, covering essential aspects such as account setup, verification, and initial configuration. 

[Watch the video on YouTube](https://www.youtube.com/watch?v=JFwAS_8BZvM&ab_channel=CSCORNERSunitaRai)

### Steps Covered in the Video:
1. **Account Registration**: How to sign up for AWS and input the required details.
2. **Verification Process**: Completing phone and email verification.
3. **Billing Information**: Adding payment details while ensuring you're on the free tier.
4. **Initial Setup**: Navigating the AWS Management Console and understanding the dashboard.


## Environment Set up

This section is for outlining steps for configuring development environment with preference to Linux OS. For that matter, we are renting an Ubuntu server on AWS and setting up the environment to suit our need.

## Step 1: Spin up EC2 Instance

* Go to AWS Console to spin up an EC2 instance of Ubuntu flavour of t3.tiny (free tier), but increase the volume to 30gb (free tier also)
* Download the secret access keys (.pem file)
* Take a note of the public IP

## Step 2: Connect to Ubuntu EC2 server

* [Optional] Move .pem secret access keys file to .ssh folder in home directory
* Execute the following command from .ssh directory to change the permission of .pem file to protect it
  ``` sh
  chmod 400 downloaded_pem_file
  ```
* Execute the following command to connect to the server
  ``` sh
  ssh -i pem_file ubuntu@public_ip
  ```
* For ease of connecting the server add the connection details in config file in .ssh directory.
  ``` sh
  nano ~/.ssh/config
  ```
* If config file is missing you can create a new one
  ``` sh
  touch config
  ```
  Add the following and save it.
  ```
  Host short-name-of-your-choice
       Hostname public-ip-of-ec2
       User ubuntu
       IdentityFile path-to-.pem-file
       StrictHostKeyChecking no
  ```
  Now run ```ssh short-name-of-your-choice``` to quickly connect to the ubuntu server. 

## Step 3: Configure Ubuntu server

### Install Anaconda

* Visit Anaconda website to get the download link for Linux (x86) version and execute the following to download the file in the server.
  ``` sh
  wget link_copied_from_anaconda_website
  ```
  for example: 
  
  ```
  wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh
  ```
* Run the following command to install the downloaded file
  ``` sh
  bash downloaded_installation_file
  ```

  for example:
  ```
  bash Anaconda3-2024.02-1-Linux-x86_64.sh
  ```
  If required, logout and re-login to the server.

### Install Docker

* Next we install Docker. However if you run into "Package ‘docker.io’ has no installation candidates" error just update the system first and then try installing again.
  ``` sh
  sudo apt update
  sudo apt install docker.io
  ```
* To run without sudo, add your user to the docker user group.
  ``` sh
  sudo groupadd docker
  sudo usermod -aG docker $USER
  ```

### Install docker-compose

* Create a separate directory to install docker compose.
  ``` sh
  mkdir soft
  cd soft
  ```
* To install Docker Compose get the latest release version for your OS (https://github.com/docker/compose -> Releases -> Assets) and make it executable.
  ``` sh
  wget link-from-docker-compose-github -o docker-compose
  chmod -x docker-compose
  ```
* To ensure docker-compose is called from soft directory from wherever we call, we need to edit .bashrc file.
  ``` sh
  nano ~/.bashrc
  export PATH="${HOME}/soft:${PATH}"
  source ~/.bashrc
  ```
  If required, logout and re-login to the server.

### Run Docker

* To verify that docker setup the following should run successfully.
``` sh
  docker run hello-world
```

### VS Code Setup

* Install "Remote - SSH" extension in VS Code
* Click on "Open a Remote Window" icon on bottom-left corner
* From dropdown select "Connect to Host" and then select Linux. That opens a new VSCode window.
* In terminal clone MLOps-Zoomcamp project
  ``` sh
  git clone https://github.com/DSML-bootcamp/w8-nbs.git
  ```
* Create a notebooks folder
  ``` sh
  mkdir notebooks
  cd notebooks
  ```
* The notebooks are hosted on the server. However to access them locally you might need to do the port forwarding that can be easily done in VSCode. Open Ports section next to terminal in VSCode and enter 8888 as port for source and enter. This will add the port to allow traffic.
* Next open jupyter notebook
  ``` sh
  jupyter notebook
  ```

**Voila!!! You are done with the configuration**

**Important**: Do not forget to stop the EC2 instance if not in use.
