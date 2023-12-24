
# Cubewave


[![made-with-Go](https://img.shields.io/badge/Made%20with-Go-1f425f.svg)](https://go.dev/)
[![Website shields.io](https://img.shields.io/website-up-down-green-red/http/shields.io.svg)](http://cubewave.dev/)

[![Twitter](https://img.shields.io/twitter/url/https/twitter.com/cloudposse.svg?style=social&label=Follow)](https://twitter.com/cubewave_dev)
[![Discord](https://badgen.net/badge/icon/discord?icon=discord&label)](https://discord.gg/dEFaa5uf6g)

## What is Cubewave?

Cubewave is a platform designed to help developers create and manage micro vms. Our tools make deploying microVMs through Firecracker simpler, minimizing the complexities entailed in the process.
We are providing two devtools: **Corewave** and **Cubeware**.

- **Corewave** operates as the equivalent of "docker-compose" for microVMs, enabling the configuration and creation of microVMs through a YAML file.

- **Cubeware** is an orchestrator, it automates the deployment and scaling of micro vms.

## Requirements:

### KVM requirements:

In order to create micro vm, you need to make sure KVM is enabled on your machine

```lsmod | grep kvm```

### OS requirements:

Linux/ubuntu

# Corewave:
## Installation

```
    wget --quiet -O - https://repo.cubewave.dev/cubewave-pubkey.asc | sudo tee /etc/apt/keyrings/cubewave-pubkey.asc 
    echo "deb [signed-by=/etc/apt/keyrings/cubewave-pubkey.asc arch=$( dpkg --print-architecture )] https://repo.cubewave.dev $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/apt-cubewave.list
    apt update
    apt install corewave
```

To verify that corewave is successfully installed run `corewave version`

you should get something like this: ```corewave v.0.1```


## Running your first MicroVM:

### YAML file structure:

Corewave requires a YAML files that defines the configuration of the MicroVM(s) you want to deploy, below is how the file should be structured:
```yaml
version: "1"
specs:
  MyApp1:
    logsPath: "FC_MyApp.log"
    CPU: 1
    RAM: 1024

  MyApp2:
    ...

  MyApp3:
    ...
```

`version`: Configuration version

`spec`: Specs block, within we specify the configuration of all our MicroVMs

`MyApp1`: Name of the MicroVM to provision (you can name it whatever you want)

`logsPath`: Path of the Firecracker log file

`CPU`: Number of host CPUs to allocate to the MicroVM

`RAM`: Amount of RAM to allocate to the MicroVM (in MB)


### Running corewave against the YAML file

Once your YAML file is ready you run Corewave and pass it the configuration file using the `-f` flag:

```
sudo corewave -f config.yml
```
**Note:** For now Corewave needs to run with privileged permissions so please make sure you are either root or using sudo when running Corewave

Corewave will then generate the necessary infrastructure components on your host VM and run firecracker with the required configuration to provision your MicroVMs.