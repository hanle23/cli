# swarm join

<!---MARKER_GEN_START-->
Join a swarm as a node and/or manager

### Options

| Name                                  | Type        | Default        | Description                                                                   |
|:--------------------------------------|:------------|:---------------|:------------------------------------------------------------------------------|
| [`--advertise-addr`](#advertise-addr) | `string`    |                | Advertised address (format: `<ip\|interface>[:port]`)                         |
| [`--availability`](#availability)     | `string`    | `active`       | Availability of the node (`active`, `pause`, `drain`)                         |
| [`--data-path-addr`](#data-path-addr) | `string`    |                | Address or interface to use for data path traffic (format: `<ip\|interface>`) |
| [`--listen-addr`](#listen-addr)       | `node-addr` | `0.0.0.0:2377` | Listen address (format: `<ip\|interface>[:port]`)                             |
| [`--token`](#token)                   | `string`    |                | Token for entry into the swarm                                                |


<!---MARKER_GEN_END-->

## Description

Join a node to a swarm. The node joins as a manager node or worker node based upon the token you
pass with the `--token` flag. If you pass a manager token, the node joins as a manager. If you
pass a worker token, the node joins as a worker.

## Examples

### Join a node to swarm as a manager

The example below demonstrates joining a manager node using a manager token.

```console
$ docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-7p73s1dx5in4tatdymyhg9hu2 192.168.99.121:2377
This node joined a swarm as a manager.

$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
dkp8vy1dq1kxleu9g4u78tlag *  manager2  Ready   Active        Reachable
dvfxp4zseq4s0rih1selh0d20    manager1  Ready   Active        Leader
```

A cluster should only have 3-7 managers at most, because a majority of managers must be available
for the cluster to function. Nodes that aren't meant to participate in this management quorum
should join as workers instead. Managers should be stable hosts that have static IP addresses.

### Join a node to swarm as a worker

The example below demonstrates joining a worker node using a worker token.

```console
$ docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-1awxwuwd3z9j1z3puu7rcgdbx 192.168.99.121:2377
This node joined a swarm as a worker.

$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
7ln70fl22uw2dvjn2ft53m3q5    worker2   Ready   Active
dkp8vy1dq1kxleu9g4u78tlag    worker1   Ready   Active        Reachable
dvfxp4zseq4s0rih1selh0d20 *  manager1  Ready   Active        Leader
```

### <a name="listen-addr"></a> `--listen-addr value`

If the node is a manager, it will listen for inbound swarm manager traffic on this
address. The default is to listen on 0.0.0.0:2377. It is also possible to specify a
network interface to listen on that interface's address; for example `--listen-addr eth0:2377`.

Specifying a port is optional. If the value is a bare IP address, or interface
name, the default port 2377 will be used.

This flag is generally not necessary when joining an existing swarm.

### <a name="advertise-addr"></a> `--advertise-addr value`

This flag specifies the address that will be advertised to other members of the
swarm for API access. If unspecified, Docker will check if the system has a
single IP address, and use that IP address with the listening port (see
`--listen-addr`). If the system has multiple IP addresses, `--advertise-addr`
must be specified so that the correct address is chosen for inter-manager
communication and overlay networking.

It is also possible to specify a network interface to advertise that interface's address;
for example `--advertise-addr eth0:2377`.

Specifying a port is optional. If the value is a bare IP address, or interface
name, the default port 2377 will be used.

This flag is generally not necessary when joining an existing swarm. If
you're joining new nodes through a load balancer, you should use this flag to
ensure the node advertises its IP address and not the IP address of the load
balancer.

### <a name="data-path-addr"></a> `--data-path-addr`

This flag specifies the address that global scope network drivers will publish towards
other nodes in order to reach the containers running on this node.
Using this parameter it is then possible to separate the container's data traffic from the
management traffic of the cluster.
If unspecified, Docker will use the same IP address or interface that is used for the
advertise address.

### <a name="token"></a> `--token string`

Secret value required for nodes to join the swarm

### <a name="availability"></a> `--availability`

This flag specifies the availability of the node at the time the node joins a master.
Possible availability values are `active`, `pause`, or `drain`.

This flag is useful in certain situations. For example, a cluster may want to have
dedicated manager nodes that are not served as worker nodes. This could be achieved
by passing `--availability=drain` to `docker swarm join`.


## Related commands

* [swarm ca](swarm_ca.md)
* [swarm init](swarm_init.md)
* [swarm join-token](swarm_join-token.md)
* [swarm leave](swarm_leave.md)
* [swarm unlock](swarm_unlock.md)
* [swarm unlock-key](swarm_unlock-key.md)
* [swarm update](swarm_update.md)
