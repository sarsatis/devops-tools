# Docker

docker system prune -a
docker container rm $(docker container ls -aq)
docker run -P imagename (Allocates host port automatically)
docker run -P -e MYSQL_ROOT_PASSWORD=mypass imagename (To pass env to container)
kubectl config delete-context gke_fsi-retailbanking-dev_us-central1_infratest-gke
kubectl config delete-cluster gke_fsi-retailbanking-dev_us-central1_infratest-gke

# Bash

uptime
free -m to check cpu utilisation
df -h to check disk utilisation

if you want to make variable permanent for everyone add  
export VARIABLE=VARIABLE_VALUE into /etc/profile file
or else add
export VARIABLE=VARIABLE_VALUE into .bashrc file

for one server to resolve another server by name you have to add
ip and name entries in /etc/hosts file like below

IP NAME

# VIM

:se num :- to set line numbers

# To change hostname on a system

vi /etc/hostname and edit the hostname and run below command
hostname name and logout and login

# Vagrant

vagrant init boxname
vagrant box list
vagrant status
vagrant reload
vagrant destroy (To destroy all vms)
vagrant halt  (To power off all vms)
vagrant up  (To bring the powerd off vms)
vagrant global-status  (To check all vms)
vagrant global-status --prune
