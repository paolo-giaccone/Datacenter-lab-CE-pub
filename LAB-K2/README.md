# LAB-K2. Basics on Kathara

Aim of this lab is (i) to learn the basic usage of [Kathara](https://www.kathara.org) to emulate simple networks and (ii) to review basic IPv4 routing. 
>[!NOTE]
>The folders related to the official repository labs of Kathara are available under the local `Kathara-Labs` folder on the VM. 
All the activities for the class labs are instead available under the  local `Datacenter-Lab-CE` folder, in order to simplify the copy&paste actions. All the folder references present in the following text refer to such local folder.

0. Open a terminal and change the folder `cd Datacenter-lab-CE`. Update the lab activity with `git pull`.

1. Type `kathara check` from command line and check that the message `Container run successfully` appears. 
2. Read carefully the slides in the folder `LAB-K2/K2-intro/001-kathara-introduction.pdf`
   * Change the default docker image (sl.15) by running `kathara settings`
   * Test kathara (sl.22)
   * Understand the meaning of ``kathara lstart``, ``kathara lclean``, ``kathara lrestart``, ``kathara linfo``, `kathara list`, `kathara wipe` commands
   * Understand the meaning of `lab.conf` file (sl.25-26) and `.startup` file (sl.30)
   * Within a container, create a file (e.g., using `touch prova.txt`) and copy into `/shared` folder. Make sure you can access it from any other container and from the local file system.
3. Enter `LAB-K2/K2-TwoHosts` 
   * Read the `lab.conf` and `.startup` files. 
   * Draw the topology 
   * Q1. What are the IP addresses?
   * Q2. What are the routing tables?
   * Q3. What is the current states of images and running containers, before starting kathara? Show the outputs of the proper `docker` commands.
   * Start the lab with `kathara lstart`
   * Q4. What is the current states of images and running containers, after starting kathara? Show the outputs of the proper `docker` commands.
   * Q5. Use `ip` command (see the reference table below) to show the IP addresses of the interfaces and the routing tables, for both hosts? 
   * Q6. Using `ping`, check the connectivity among the two hosts and report the output
   * Q7. What is the output of `kathara linfo` when the lab is running?
   * Q8. What is the output of `kathara list` when the lab is running?
   * Now stop the lab using `kathara lclean`.
   * Q9. What is the current states of images and running containers, after starting kathara? Show the outputs of the proper `docker` commands.

## Reference commands

* NET is an network prefix (e.g., `10.1.2.0/24`)
* IP is an IP address of an interface (e.g., `10.1.2.1`)
* IP/MASK is an IP address with network mask MASK of an interface (e.g., `10.1.2.1/24`)
* IFACE is an interface id (e.g., `eth0`)

| Command | Meaning |
|----| ----|
| Interface settings|
| `ip -4 address`| Diplay IPv4 info on all the interfaces|
| `ip address add IP/MASK dev IFACE` | Configure IP address on an interface|
| Routing table settings |
| `ip route list`| Show routing tables |
| `ip route add NET dev IFACE` | Add route to reach a network prefix on direct delivery on the given interface|
| `ip route add NET via IP`| Add route to reach a network prefix through the specified gateway|
| `ip route add default via IP` | Add the default  gateway|
| `ip route del NET dev IFACE`| Remove route |
| `ip route del NET via IP`| Remove route|
| `ip route del default` | Remove the default gateway|



   


