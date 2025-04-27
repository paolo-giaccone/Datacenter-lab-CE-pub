# LAB-K3. Basic topology and routing

## 1. Single server topology
Configure a topology in which router `r1` is connected to host `hb` on one
interface and to host `bg` on the other interface. 

![Net3](Figs/net3.drawio.png)

Create a `lab.conf` file:

```shell
ha[0]="A"
ha[image]="kathara/base"

r1[0]="A"
r1[1]="B"
r1[image]="kathara/frr"

hb[0]="B"
hb[image]="kathara/base"
```
thus, `ha` and `hb` are connected on the same network.


Configure `ha`. Create a `ha.startup` file:
```shell
ip address add 192.168.1.1/24 dev eth0

ip route add default via 192.168.1.128 dev eth0
```

Configure `hb`. Create a `hb.startup` file:
```shell
ip address add 192.168.2.1/24 dev eth0

ip route add default via 192.168.2.128 dev eth0
```

Configure `r1`. Create a `r1.startup` file:
```shell
ip address add 192.168.1.128/24 dev eth0
ip address add 192.168.2.128/24 dev eth1
```
Run the lab through `kathara lstart` and test connectivity and performance

**Q1.1** Through `ip address`, report the IP address for all the interfaces (excluded the local loop).

**Q1.2** Through `ip route`, report the routing tables for `ha`, `hb` and `r1`.

**Q1.3** Report the three routing tables according to the following scheme:
| Network prefix | Gateway | Interface |
|---|---|---|
| ... |   |   |


**Q1.4** Through `ping`, report the output of the connectivity test between: `ha-r1`, `hb-R1`, `ha-hb`.

**Q1.5** Through `traceroute`, report the output of the route `ha->hb` and of the route `hb->ha`. Are the same? Why?

**Q1.6** Through `iperf3`, report the average bandwidth between `ha` and `hb`. Recall that `iperf3 -s` runs as server and `iperf3 -c X.X.X.X` runs as client sending the traffic towards `X.X.X.X`. 

## 2. Linear topology

Consider the topology below.

![Net1](Figs/net1.drawio.png)


**Q2.1** Choose a proper addressing plan in order to minimize the waste of IP addresses, within the range 10.0.0.0/8. Assume that at most 1000 hosts could be connected to each network *neta1* and *netbB*. Fill the following table.


| Network | Network address|
| ---| ---|
| neta1  ||
|net12  |   |
| net2b| |

| Interface | IP address/netmask |
|---|--- |
| ha | |
| hb
| r1a | |
| r12 | |
| r21 | |
| r2b | |

**Q2.2** Configure the routing tables for each device. Fill the following table.

| Network prefix | Gateway | Interface |
|---|---|---|
|  ... |  |  |

**Q2.3** Show the routing path `ha->hb` and `hb->ha` through `traceroute`.

## 3. Routing in a loop topology

Consider the topology below.

![Net1](Figs/net2.drawio.png)

**Q3.1** Choose a proper addressing plan in order to minimize the waste of IP addresses, within the range 10.0.0.0/8. Assume that at most 100 hosts could be connected to each network *neta1* and *net4b*. Fill the following table.

| Network | Network address|
| ---| ---|
| neta1  ||
|net12  |   |
|net24  |   |
|net13  |   |
|net34  |   |
| net4b| |

| Interface | IP address/netmask |
|---|--- |
| ha | |
| hb||
| r1a ||
| r12 | |
| r13 | |
| r21 ||
| r24 ||
| r31 ||
| r34||
| r42 ||
|r43||
|r4b||


**Q3.2** Configure the routing tables for each device such that *the traffic follow a clockwise direction within the loop* inside the topology. Fill the following table.

| Network prefix | Gateway | Interface |
|---|---|---|
|  ... |  |  |

**Q3.3** Show the output of `ping ` from `ha` to `hb` and vice versa.

**Q3.4** Show the output of `traceroute` for path `ha->hb` and for path `hb->ha`.

**Q3.5** (Optional) Show a routing table (as similar as possible to **Q3.2**) that would lead to a routing loop. For which destination IPs a routing loop will occur? 

**Q3.6** (Optional) Configure the routing table as in Q3.5 and show the effect of a routing loop using `ping` and `traceroute`. 



 