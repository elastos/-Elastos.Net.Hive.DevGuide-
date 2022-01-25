# Deploy as Docker Containers

With the continuous maturity of Docker technology, service deployment based on the Docker environment has become a standardized deployment method. Hive Node first provides a Docker-based deployment mode. Because it contains three sub-services, three Docker Container service processes will be formed after the Docker deployment.

## Requirements

In principle, Hive Node can be deployed in any Linux system, but currently, the Hive Node team only verifies in Ubuntu Linux 22.04 environment. Thus, please try to consider deploying and running in the Ubuntu Linux environment when community users install Hive Node. Docker and Python 3 are needed to deploy Hive Node. The installation steps are as follows:

### 1. Prerequisites Installed (Docker and Python3)

Assumed the Docker and Python3 already shipped on your system. Otherwise, you need to install them manually. Generally, there is python3.6 already built inside Ubuntu 22.04. To Docker, you can run the following commands to install it:

```shell
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
$ sudo usermod -aG docker your-userid
```

_**Notice:**_ _you will need to sign out from the target server and log in again to make Docker work effectively._

### 2. Download Package and Unzip

Download the latest version of packages and untar it onto your server. At the time of this writing, you can run the commands as below:

```shell
$ curl -fsL https://github.com/elastos/Elastos.NET.Hive.Node/archive/release-v2.0.1.tar.gz -o release.tar.gz
$ tar -xzvf release.tar.gz 
```

### 3. Start the Installation

Enter into the source code folder and directly run the command to install the Hive node service:

```shell
$ /bin/bash -c "$(curl -fsSL https://www.trinity-tech.io/hivenode/scripts/run.sh)" -s docker
```

After the above steps have been finished, use the following command to check whether the Hive Node functions or not. If successful, it will display two container instances. One is hive-node, and the other is MongoDB.

```shell
$ docker ps
```

### 4. Run the test-cases

Run the next command to launch test-cases to verify the deployed node at the same device:

```shell
./run.sh test
```

## Deployment

At present, Hive Node automatically installs all Hive Node sub-services through scripted operation, and this script comes from the source code root directory of Hive Node.

```shell
$ ./run.sh docker
```

This command will do the following:

* Run MongoDB service and IPFS service with the mode of docker. If there is no image of MongoDB and IPFS, it will be downloaded automatically.
* Run the Hive Node service with the mode of docker, which will create the image of Hive Node locally through the downloaded source code.
* Execute the detection script to ensure that the Hive Node service starts normally so that it can be accessed later.
