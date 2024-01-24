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


