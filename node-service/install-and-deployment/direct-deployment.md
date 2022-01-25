# Direct Deployment

Hive Node also supports installation in normal installation mode.

## Installation

At present, Hive Node automatically installs all Hive Node sub-services through scripted operation:

```shell
$ ./run.sh direct
```

This command will do the following:

* Run MongoDB service and IPFS service with the mode of docker.
* Run Hive Node service directly.
* Execute the detection script to ensure that the Hive Node service starts normally.
