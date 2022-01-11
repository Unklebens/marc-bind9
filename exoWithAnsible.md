fichier ansible host

all:
  children:
    puppet:
      vars:
        ansible_ssh_common_args: '-o StrictHostKeyChecking=no'

      hosts:
        puppetserver:
          ansible_host: 192.168.10.24
          hostname: puppetserver.localdomain.fahim
  
        puppetclient:
          ansible_host: 192.168.10.25
          hostname: client.localdomain.fahim
		  
        dnsserver:
          ansible_host: 192.168.10.26
          hostname: dnsserver.localdomain.fahim
		  

commande a appliquer sur le ansible host


abc@3cfef058b46a:~/workspace/ansible$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/config/.ssh/id_rsa): puppet
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in puppet
Your public key has been saved in puppet.pub
The key fingerprint is:
SHA256:rDeO+SIbplZLWWVGHgrxPknlxLGaxRksnMG28x+rnHw abc@3cfef058b46a
The key's randomart image is:
+---[RSA 3072]----+
|    o+.*B.       |
|     o*B*=       |
|     .+*B        |
|     o+*         |
|     o*oS        |
|    +  o. .      |
|   oo.. o. o     |
|  .oo..B oE      |
| .. .oooBo       |
+----[SHA256]-----+

sudo apt-get update -y
sudo apt install python3-pip -y
pip3 install ansible
sudo apt install sshpass -y

commande pour les hosts ansible

useradd centos
passwd centos

# verification de la possibilité de l'application de playbooks

update.yaml

```
---
- name: "update"
  hosts: all
  tasks:
    - name: "update taskj"
      ansible.builtin.command: "yum update -y"
```


lancement du playbook


```
abc@3cfef058b46a:~/workspace/ansible$ ansible-playbook -i puppetExoMarcHosts.yaml update.yaml 

PLAY [update] *****************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************
ok: [dnsserver]
ok: [puppetclient]
ok: [puppetserver]

TASK [update taskj] ***********************************************************************************************************
changed: [dnsserver]
changed: [puppetclient]
changed: [puppetserver]

PLAY RECAP ********************************************************************************************************************
dnsserver                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
puppetclient               : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
puppetserver               : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
