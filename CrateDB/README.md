# CrateDB Setup:
As a distributed database, CrateDB requires multi-node setup. For this purpose, we, initially, install CrateDB in all hosts of our system with a single-node setup, modifying subsequently it's "main" configuration file to convert it to a multi-node one.

To begin with, we ensure that the Advanced Packaging Tool (apt) of Ubuntu  is updated and that all it's required packages are installed, as follows:
```bash
sudo apt update
sudo apt install --yes apt-transport-https apt-utils curl gnupg lsb-release
```
Next, we download the public GPG key, for the OS to authenticate the CrateDB files once we request it's installation:
```bash
curl -sS https://cdn.crate.io/downloads/deb/DEB-GPG-KEY-crate | sudo tee /etc/apt/trusted.gpg.d/cratedb.asc
```
We locate the repository that contains CrateDB and we include it to the list of our system's repositories of package manager apt, as follows:
```bash
[[ $(lsb_release --id --short) = "Debian" ]] &&repository="apt"
[[ $(lsb_release --id --short) = "Ubuntu" ]] &&repository="deb"
distribution=$(lsb_release --codename --short)

echo "deb [signed-by=/etc/apt/trusted.gpg.d/cratedb.asc arch=amd64] https://cdn.crate.io/downloads/${repository}/stable/ ${distribution} main" \
| sudo tee /etc/apt/sources.list.d/cratedb.list
```
At this point, the apt will have access to the reporitories' packages of CrateDB and our system's files (cratedb.list, cratedb.asc) will have all the necessary information for the CrateDB to be installed and hosted in the node through the package manager apt, using the following commands:
```bash
sudo apt update
sudo apt install crate
```
**!** As mentioned earlier, the abovementioned process must be executed in every node of our cluster. At this point the single-node installation of CrateDB must have been successfully completed, which can be verified by executing the following:
```bash
sudo systemctl start crate # start the database
sudo systemctl stop crate # stop the database
sudo systemctl restart crate # restart the DB
sudo systemctl status crate # check the status of the DB
```
