# Comparison of CrateDB and QuestDB Time Series DBs
Term project for the course 'Analysis and Design of Information Systems', NTUA, 2023/24

## Objective:
This project was carried out as the chosen term project for the course of "Analysis and Design of Information Systems" at the School of Electrical and Computer Engineering of the National Technical University of Athens. The objective is a comparative study of QuestDB and CrateDB, two Time-Series DBs - databases oriented and adapted for managing time-series data -, and includes the installation and setup of the system, that will host the two databases, and of the databases themselves, the data generation and insertion into the two DBs and the query generation and execution. With a view to compare the databases, suitable performance measurements were carried, and namely data insertion and query execution time, thoughput, latency, cpu usage and node/cluster load (cluster load only for CrateDB, the only distributed DB amongst the two), whose results are attempted to be explained and justified based on the structural characteristics of the two databases. Ultimately, conclusions are drawn regarding the conditions under which the use of each of the two databases under consideration is most appropriate.

## Collaborators:
- [Dimitrios-David Gerokonstantis](https://github.com/DimitrisDavidGerokonstantis)  (el19209)
- [Athanasios Tsoukleidis-Karydakis](https://github.com/ThanosTsoukleidis-Karydakis)  (el19009)
- [Filippos Sevastakis](https://github.com/FilipposSevastakis) (el19183)

## Cluster setup:
For the implementation of the project we were, initially, required to configure a cluster of interconnected machines, since not only do we intent to host multiple databases, but, most significantly, one of those is a distributed one. To that end, we were provided 3 VM's by okeanos-knossos, an Infrastructure-as-a-Service (IaaS) of the Greek Research and Technology Network (GRNET), that we set up executing the following:

1) Private-Public keys' creation:<br>
   To establish access from our "personal" machines to the virtual machines of okeanos-knossos we need to provide okeanos with a public key (per personal machine). For this purpose, we execute the following command in our machines' terminal:
   ```bash
   ssh-keygen
   cat ~/.ssh/id_rsa
   ```
   With the latter command we print a newly-created public key, which we can then register in the okeanos service (via it's website).
2) Virtual Machines creation:<br>
   We create the 3 machines selecting 4 CPUs, 8GB RAM, 30GB disk space and Ubuntu 16.04 LTS operating system (which we will subsequently upgrade) for each. We also ensure that we use the newly created public keys for the machines to recognize. Access to the machines should be now able by executing the following command:
   ```bash
   ssh user@snf-****-ok-kno.grnetcloud.net
   ```
   where "snf-****-ok-kno.grnetcloud.net" corresponds to a specifiv machine created.
3) Private Network configuration:<br>
   At this point, a public network should be set up in order for the VM's (having the role of 'master' or 'worker') to communicate with each other. To this end, we do the following configurations:
   - We optionally, for our convenience, rename the VM's from each one's terminal as follows:
     ```bash
     sudo hostnamectl set-hostname okeanos-ISmaster
     sudo hostnamectl set-hostname okeanos-ISworker1
     sudo hostnamectl set-hostname okeanos-ISworker2
     ```
   - We create a new private network in the "network" section of the okeanos UI, to which we attach all 3 VM's. In our case the IPv4 addresses given were 192.168.1.2 (master), 192.168.1.3 (worker1) and 192.168.1.4 (worker2), which we use to modify the /etc/hosts file in each VM in order to connect all hosts with each other as follows:
     ```bash
     127.0.0.1     localhost
     192.168.1.2   okeanos-ISmaster
     192.168.1.3   okeanos-ISworker1
     192.168.1.4   okeanos-ISworker2

     # The following lines are desirable for IPv6 capable hosts
     ::1   localhost ip6-localhost ip6-loopback
     ff02::1         ip6-allnodes
     ff02::2         ip6-allrouters
     ```
     We, then, reboot all hosts
   - At this point, we should establish authorized access from one host to another. For this purpose, we execute the commands below to create a public-private-key pair in master's VM, concatenate the public key to it's ./ssh/authorised_keys file and, also, output that key to the terminal:
     ```bash
     ssh-keygen -t rsa
     cat ∼/.ssh/id_rsa.pub >> ∼/.ssh/authorized_keys
     cat ∼/.ssh/id_rsa.pub
     ```
     We copy the output and paste it in each workers' ./ssh/authorized_keys file and can now connect from one host to another by executing `ssh <hostname>`.
4) Operation System upgrade:<br>
   We gradually upgrade the operation system of all 3 VMs from Ubuntu 16.04 LTS to Ubuntu 20.04 LTS (the latest Ubuntu version that both Databases maintain packages for).
   - We initially upgrade to Ubuntu 18.04 by executing `sudo apt update && sudo apt upgrade -y`. When asked, we choose _"Install the package maintainer's version"_ and after the completion of the installation we reboot the VM (`sudo reboot`) and execute `sudo do-release-upgrade`, choosing again when asked _"Install the package maintainer's version"_, as well as _"dev/vda"_ as the GRUB install device.
   - For the upgrade from Ubuntu 18.04 to 20.04 we execute `sudo do-release upgrade`, choosing _"yes"_ to all options and _"4.0"_ as the upgrade version of LXD snap, when asked.

   :warning: Possible Issues you may encounter (esp. using okeanos):<br>
   In case of an inability to upgrade the operating system or generally in situations where it seems that the system is experiancing connectivity issues with the "outside" world, it is recommended to check the routing table using the `netstat -nr` command. If an address in the form of 192.168.1.1 appears as the first address insted of the defaul gateway of the public network (83.212. ...), try to delete it executing `sudo route delete default gw 192.168.1.1 eth1`.

![Selection_001](https://github.com/FilipposSevastakis/InformationSystems_TermProject/assets/106911339/09703bd0-78d3-4896-b7e5-724c4a30cb77)
