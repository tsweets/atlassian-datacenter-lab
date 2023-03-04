# What is this?
My team and I are working on an Atlassian Datacenter upgrade from Server - [cuz reasons](https://www.atlassian.com/blog/platform/atlassian-server-is-going-away-next-steps)

Being in an air gapped environment makes it really difficult for us to try out new things, therefore I tend to do a bunch of testing outside of my work environment. This is one such test, in which I create an entire Atlassian Datacenter Lab with VMs on my home Workstation.

Since this is a test lab, I don't really care about any password leaks, as they will never be used for reals. 

### Plan
Install, configure, and test out DC in an Internet Connected environment 1st. This will allow me to figure out the pieces I need to request (I have to request and have screened every single piece of COTS/FOSS). I will do this via creating an "Ansibilized" test environment and this is what this project provides. 

### Dev Environment
- Beefy Workstation (Fedora 37, AMD 5950x, 128GB RAM)
- Ansible
- Docker
- VMWare Workstation Pro (this just works better than libvirt and VirtualBox IMHO)
- Molecule  (https://molecule.readthedocs.io/en/latest/installation/)  

### Anisble Lab  
Here's a list of VMs I have created
```
10.220.1.79 labsupport labsupport.atlassian.rhel.lab
10.220.1.80 crowd crowd.atlassian.rhel.lab
10.220.1.81 jira jira.atlassian.rhel.lab
10.220.1.82 confluence confluence.atlassian.rhel.lab
10.220.1.83 bitbucket bitbucket.atlassian.rhel.lab
10.220.1.84 bamboo bamboo.atlassian.rhel.lab
10.220.1.85 bamboo-agent01 bamboo-agent01.atlassian.rhel.lab
10.220.1.86 bamboo-agent02 bamboo-agent02.atlassian.rhel.lab
```

Each VM has 2 CPUS, 2GB RAM, and 25GB Disk. Also each one is CentOS RHEL 8 Stream (We actually use RHEL but this is close enough)


#### Anisble Notes:
##### Create new role
```
molecule init role beer30.k3s -d podman
```

##### Collections Installed
ansible-galaxy collection install community.postgresql  
needed dnf install psycopg2 on the host - but this is in the playbook

ansible-galaxy collection install community.crypto

ansible-galaxy collection install community.general

Jira doesn't work with the stock open jdk - had to install   https://adoptium.net/temurin/releases/?version=11  



##### Molecule Cheat Sheet
https://molecule.readthedocs.io
- molecule create
- molecule converge
- molecule login
- molecule destroy

##### Run Playbook
ansible-playbook -i hosts.ini bootstrap.yml



## Notes:

Shared NFS  
mount -t nfs nas.rhel.lab:/volume1/atlassian /mnt/atlassian-datacenter  
or ip 10.107.0.15

Helm Installs  
helm install --generate-name  atlassian-data-center/crowd  --values values.yaml 


