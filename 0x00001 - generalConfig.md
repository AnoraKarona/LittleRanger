Configurações gerais do LittleRanger. As configurações de user devem ser feitas antes de configurar outros serviços do server.

* Criação de user e permissões (trocar permissões)

# useradd karona -m
# passwd karona
# vim /etc/sudoers
karona ALL=(ALL) ALL


* Definindo IP

# vim /etc/network/interfaces

auto eth0
iface eth0 inet static
address 192.168.X.X
netmask 255.255.255.0
gateway 192.168.X.X

# /etc/init.d/networking restart


* Definindo hostname

# vim /etc/hostname

LittleRanger