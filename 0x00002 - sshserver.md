* Servidor SSH(Secure Shell) and RSA.

 Utilizado para acessar o LittleRanger pelo RainhaAna remotamente e apenas por ele o acesso via SSH.

 -ssh - Client
 -sshd - Server
 -scp - Programa para transferência de arquivos
 -ssh-keygen - Gerar chaves para SSH
 -sftp - Client ftp
 -sftp-server - Server ftp
 -ssh-add - Adiciona chaves de autenticação RSA ou DSA
 -ssh-agent - Agent de autenticação, armazena a private key para autenticação via public key


* Arquivos de configuração:

 -/etc/ssh/sshd_config - Arquivo de configuração do server SSH
 -/ect/ssh/ssh_config - Arquivo de configuração do client SSH
 -~/.ssh/config - Arquivo de configuração pessoal do client SSH



* Client - Gerando key copiando para o server

# ssh-keygen -t rsa -b 4096

Generating public/private rsa key pair.
Enter file in which to save the key (/home/karona/.ssh/id_rsa):
Created directory '/home/karona/.ssh'
Enter passphrase (empty for no passphrase): ********
Enter same passphrase again: ********
Your identification has been saved in /home/karona/.ssh/id_rsa.
Your public key has been saved in /home/pi/.ssh/id_rsa.pub.
The key fingerprint is:
...
The key's randonart image is:
...

# ssh-copy-id karona@192.168.X.X


* Server - Instalação e configuração do Server SSH

# apt install -y openssh-server
# cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bkp
# vim /etc/ssh/sshd_config

Port 2220
AllowUsers karona
PermitRootLogin no
PasswordAuthentication no
Banner /home/karona/banner

# systemctl restart sshd
ou
# /etc/init.d/ssh restart


* Client - Login SSH

$ ssh -p 2220 192.168.X.X


References:
https://www.openssh.com/manual.html
https://guiafoca.org/guiaonline/avancado/ch15.html
https://wiki.archlinux.org/index.php/OpenSSH
