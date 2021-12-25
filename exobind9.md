# creation des machines en local via vitualbox 

## Centos 7-minimal

### Connexion par pont **pour etre sur le meme reseau que la machine hote**

![Screenshot](asset\centosdns.PNG)

sur toutes les machines 

```
echo "127.0.0.1 localhost
192.168.10.13 puppetserver.localdomain.fahim
192.168.10.12 client.localdomain.fahim
192.168.10.9 dnsserver.localdomain.fahim" > /etc/hosts
echo "${HOSTNAME}/localadmin.fahim" > /etc/hostname
reboot now
```


# Puppetserver

```
sudo rpm -Uvh https://yum.puppet.com/puppet7-release-el-7.noarch.rpm
sudo yum update -y
sudo yum install puppetserver -y
sed -i 's/-Xms2g -Xmx2g/-Xms1g -Xmx1g/' /etc/sysconfig/puppetserver

echo "# This file can be used to override the default puppet settings.
# See the following links for more details on what settings are available:
# - https://puppet.com/docs/puppet/latest/config_important_settings.html
# - https://puppet.com/docs/puppet/latest/config_about_settings.html
# - https://puppet.com/docs/puppet/latest/config_file_main.html
# - https://puppet.com/docs/puppet/latest/configuration.html

[master]
dns_alt_names = puppetserver.localdomain.fahim

[main]
certname = puppetserver.localadmin.fahim
server = puppetserver.localdomain.fahim
environment = production
runinterval = 1h" > /etc/puppetlabs/puppet/puppet.conf

```

## client puppet (puppetslave de base) & client dns

client.localdomain.fahim

```
echo "# This file can be used to override the default puppet settings.
# See the following links for more details on what settings are available:
# - https://puppet.com/docs/puppet/latest/config_important_settings.html
# - https://puppet.com/docs/puppet/latest/config_about_settings.html
# - https://puppet.com/docs/puppet/latest/config_file_main.html
# - https://puppet.com/docs/puppet/latest/configuration.html


[main]
certname = client.localdomain.fahim
server = puppetserver.localdomain.fahim
environment = production
runinterval = 1h" > /etc/puppetlabs/puppet/puppet.conf
```

## Clients puppet & dnsserver

dnsserver.localdomain.fahim

```
sudo rpm -Uvh https://yum.puppet.com/puppet7-release-el-7.noarch.rpm
sudo yum update -y
sudo yum install puppet-agent -y
sudo /opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true

echo "# This file can be used to override the default puppet settings.
# See the following links for more details on what settings are available:
# - https://puppet.com/docs/puppet/latest/config_important_settings.html
# - https://puppet.com/docs/puppet/latest/config_about_settings.html
# - https://puppet.com/docs/puppet/latest/config_file_main.html
# - https://puppet.com/docs/puppet/latest/configuration.html


[main]
certname = dnsserver.localadmin.fahim
server = puppetserver.localdomain.fahim
environment = production
runinterval = 1h
" > /etc/puppetlabs/puppet/puppet.conf
```


## verification avant signature certificats

```
cat /etc/hostname && cat /etc/hosts && cat /etc/puppetlabs/puppet/puppet.conf
```

### puppetserver

```
[root@puppetserver ~]# cat /etc/puppetlabs/puppet/puppet.conf
# This file can be used to override the default puppet settings.
# See the following links for more details on what settings are available:
# - https://puppet.com/docs/puppet/latest/config_important_settings.html
# - https://puppet.com/docs/puppet/latest/config_about_settings.html
# - https://puppet.com/docs/puppet/latest/config_file_main.html
# - https://puppet.com/docs/puppet/latest/configuration.html

[master]
dns_alt_names = puppetserver.localdomain.fahim

[main]
certname = puppetserver.localdomain.localadmin.fahim
server = puppetserver.localdomain.fahim
environment = production
runinterval = 1h
```

### puppetclient&dnsclient

```
[root@client ~]# cat /etc/hostname && cat /etc/hosts && cat /etc/puppetlabs/puppet/puppet.conf
client.localdomain.fahim
127.0.0.1 localhost
192.168.10.13 puppetserver.localdomain.fahim
192.168.10.12 client.localdomain.fahim
192.168.10.9 dnsserver.localdomain.fahim
# This file can be used to override the default puppet settings.
# See the following links for more details on what settings are available:
# - https://puppet.com/docs/puppet/latest/config_important_settings.html
# - https://puppet.com/docs/puppet/latest/config_about_settings.html
# - https://puppet.com/docs/puppet/latest/config_file_main.html
# - https://puppet.com/docs/puppet/latest/configuration.html

[main]
certname = client.localdomain.fahim
server = puppetserver.localdomain.fahim
environment = production
runinterval = 1h
```



### puppetclient&dnsserver

```
[root@dnsserver ~]# cat /etc/hostname && cat /etc/hosts && cat /etc/puppetlabs/puppet/puppet.conf
dnsserver.localdomain.fahim
127.0.0.1 localhost
192.168.10.13 puppetserver.localdomain.fahim
192.168.10.12 client.localdomain.fahim
192.168.10.9 dnsserver.localdomain.fahim
# This file can be used to override the default puppet settings.
# See the following links for more details on what settings are available:
# - https://puppet.com/docs/puppet/latest/config_important_settings.html
# - https://puppet.com/docs/puppet/latest/config_about_settings.html
# - https://puppet.com/docs/puppet/latest/config_file_main.html
# - https://puppet.com/docs/puppet/latest/configuration.html


[main]
certname = dnsserver.localadmin.fahim
server = puppetserver.localdomain.fahim
environment = production
runinterval = 1h
```


# depoiement dsn BIND9





