# Deploy as Docker containers

随着 Docker 技术的不断成熟，基于 Docker 环境的服务部署已成为一种标准化的部署方式。Hive Node 首先提供基于 Docker 的部署模式，由于其内部包含三个子服务，Docker 方式部署后会形成三个Docker Container 服务进程。

## Requirements

原则上 Hive Node 可以部署在任何 Linux 系统中，但是目前 Hive Node 团队仅在 Ubuntu Linux 22.04 环境验证，请社区用户安装 Hive Node 时尽量考虑选择在 Ubuntu Linux 中环境部署运行。部署 Hive Node 需要用到 Docker 和 Python 3，以下为安装步骤：

### 1. Prerequisites Installed (Docker and Python3)
Assumed the Docker and Python3 already shipped on your system. Otherwise, you need to install them manually. Generally, there is python3.6 already built inside Ubuntu 22.04.  To Docker, you can run the following commands to install it:

```shell
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
$ sudo usermod -aG docker your-userid
```
***Notice here:*** *you will need to sign out from the target server and log in again to make Docker work effectively.*

### 2. Download package and unzip it
Download the latest version of packages and untar it onto your server. At the time of this writing, you can run the commands as below:

```shell
$ curl -fsL https://github.com/elastos/Elastos.NET.Hive.Node/archive/release-v2.0.1.tar.gz -o release.tar.gz
$ tar -xzvf release.tar.gz 
```
### 3. Start the installation
Enter into the source code folder, directly run the command to install the Hive node service:

```shell
$ /bin/bash -c "$(curl -fsSL https://www.trinity-tech.io/hivenode/scripts/run.sh)" -s docker
```
After all the stuff has been finished, use the following command to check whether the Hive Node can work or not.  In the case of success, it will display two container instances. One is hive-node, and the other is MongoDB.
```shell
$ docker ps
```
### 4. Run the test-cases
Run the next command to launch test-cases to verify the deployed node at the same device:

```shell
./run.sh test
```

## Deployment

目前 Hive Node 通过脚本化运行自动安装所有Hive Node子服务，此脚本来自于 Hive Node 的源代码根目录。

```shell
$ ./run.sh docker
```

该命令会执行如下操作：

- 以 docker 的方式运行 mongodb 服务和 IPFS 服务，如果没有 mongodb 和 IPFS 的 image，会自动下载。
- 以 docker 的方式运行 Hive Node 服务，该操作将会在本地通过下载的源码创建 Hive Node 的 image。
- 执行检测脚本确保 Hive Node 服务正常启动，以便后续能进行访问。
