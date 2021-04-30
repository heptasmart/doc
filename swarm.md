# Swarm user guide

This project uses Docker swarms to create abstract networks used by Spark nodes

## Opened ports

Using swarms needs the following ports to be open and publicly accessible on every server/computer that will be part of the system : 

* **TCP** `2377`
* **TCP** et **UDP** `7946`
* **UDP** `4789`

## Creating the swarm

Two scenarios have been identified for creating a swarm.

* The computer has a public IP on one of its interfaces

```bash
docker swarm init --advertise-addr $PUBLIC_IP
```

* The computer is hidden behind a router

```bash
docker swarm init --advertise-addr $ROUTER_PUBLIC_IP --listen-addr $SERVER_PRIVATE_IP
```

```swarm init``` will produce an output similar to this (where ```$AD_IP``` is the IP address used in the ```--advertise-addr``` option ) :

```
Swarm initialized: current node (abcdefghijklmnopqrstuv) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx $AD_IP:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

```

The command has to be executed on any server which is needed to join the swarm.

## Creating an overlay network

The overlay network will allow a seamless communication between swarm node.

```bash
docker network create --driver=overlay --attachable spark-net
```

The ```--attachable``` tag makes the network available for future containers.

