# LAB-K4. BGP


## 4.1 Summary of the steps to emulate a network with BGP 

The main steps to follow to start a lab on BGP is the following:
1. create a `lab.conf` file with the topology and the images of each node. The router image should be `frr` as in the following example: `rX[image]="kathara/frr"` 
2. create a `rX.startup` file for each router (say `rX`), assigning the IP address for each interface. Typically, you do not have to set the routing tables, since they will be computed by the routing protocol. Add also a line with `systemctl start frr` to start the routing daemon. See the [rX.config](template/rX.startup) template
3. Create, for each router (say `rX`), a folder sequence as `rX/etc/frr/` and with all the following files:
  * a file `daemons` with all the options required to start the routing daemaons. Use the [daemons](template/rX/etc/frr/daemons) template
  * a file `vtysh.conf` to configure the shell used to interact with BGP daemon `bgpd`. In particular, you must change the hostname into `hostname rX-frr` with the proper `rX`.
  * a file `frr.conf` to configure BGP. This is the most important file to modify to control the behavior of BGP. Use the [frr.conf](template/rX/etc/frr/frr.conf) template.
4. Start the lab as usual 


## 4.3 Main BGP configuration

The syntax of `frr.conf` is the following:
- `! comment`: add a comment
- `router bgp <my-as-number>`: define the AS number of the router
- `neighbor <neighbor-ip> remote-as <neighbor-as-num>`: define XXX
- `neighbor <neighbor-ip> description <text>`: define XXX

## 4.2 Useful commands in vtysh

On the terminal of any BGP router, you can start `vtysh` to get detailed information from BGP deamon. The full documention is available on [https://docs.frrouting.org/en/latest/bgp.html](https://docs.frrouting.org/en/latest/bgp.html) under the section _Displaying BGP Information_. The main commands are the following:

- `show ip route`: routing tables
- `show ip bgp`: AS paths 
- `show ip bgp A.B.C.D` 
- `show bgp all`
- `show bgp all detail`
- `show bgp neighbors`



## 4.3 Peerring between two routers

Read the [lab activity](bgp-simple-peering/041-kathara-lab_bgp-simple-peering.pdf)
and perfom the whole activity. 

Now start a new lab in a new folder implementing the following topology:

![Net4](Figs/peering.drawio.png)

with the following IP addressing plan.

| Interface | IP address/netmask |
|---|--- |
| h1-eth0 |1.1.1.1/24 |
| h2-eth0| 3.3.3.1/24 |
| r1-eth0| 1.1.1.10/24|
| r1-eth1| 2.2.2.10/24|
| r2-eth0| 2.2.2.20/24|
| r2-eth1| 3.3.3.20/24|

Answer to the following questions:

1. Configure the proper name of the two ASs. Which file/s you modified?
2. Start the lab. Check for both routers the routing tables with both `route` and `ip route`. Are the obtained info equivalent?
1. Looking at the `log` file of BGP, how often the KEEPALIVE message is sent among the routers?
1. Instead of using `telnet`, use `vtysh` to access the command line interface of `bgpd`. What you learn from the output of `show ip bgp`? You can exit `vtysh` by typing `exit`.
1. What you learn from the output of `show ip summary`?
1. Try to ping `h2` from `h1`. Does it work? Why?

## 4.4 BGP Announcements with 2 ASs

Read the [lab activity](bgp-announcement/042-kathara-lab_bgp-announcement.pdf)
and perfom the whole activity. 

Now start a new lab in a new folder implementing the following topology:

![Net4](Figs/peering.drawio.png)

with the IP addressing plan of section 4.3.

Answer to the following questions:
1. Report the output `show ip route` within `vtysh` and the output of `route` for each router. Is the information the same? If not, what are the differences?
1. Try to ping `h2` from `h1`. Does it work? Why?
1. Consider the log file of frr in each router. Report the line of the log with announcements sent from the router and the announcements received at the router.
1. Report the AS paths in each router shown with `show ip bgp`. Fill also the following table:

| Network prefix | AS path |
|---|---|
| ... | ...|


## 4.5 BGP Announcements with a linear topology

Now start a new lab in a new folder implementing the following topology:

![Net4](Figs/peering3.drawio.png)

with the IP addressing plan of section 4.3,  plus the following interfaces:
| Interface | IP address/netmask |
|---|--- |
| r2-eth2| 4.4.4.20/24|
| r3-eth0| 4.4.4.30/24|
| r3-eth1| 5.5.5.3/24|

1. Report the routes in each router shown with `show ip route`. Are they correct? Why?
1. Try to ping `h2` and `h3` from `h1`. Does it work? Why?
1. Report the AS paths in each router shown with `show ip bgp`. Fill also the following table:

| Network prefix | AS path |
|---|---|
| ... | ...|


## 4.6 (Optional) BGP Announcements for a loop topology

Now start a new lab in a new folder implementing the following topology:

![Net4](Figs/peering4.drawio.png)

1. Configure all the IP addresses.

2. Check if BGP is able to compute properly the routing tables.

1. Report the routing tables with `show ip route`. Is there any routing loop?

1. Report the AS paths with `show ip bgp`. Are all the paths considered.

1. Comment about the capability of BGP of managing routing loops.


