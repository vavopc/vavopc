

Instalação do OCS Inventory (Ultima versão, 2.9.2 - 2022) no Debian 11.


	OCS – Open Computer and Software Next Generation, nada mais é que um software livre usado para Infraestrutura de TI, para fazer inventários do parque de TI. È possível fazer o levantamento de softwares, hardwares eativos de rede como até Smartphone. Ele funciona de forma web, sendo necessário apenas um servidor em linux web com poucos recurso ou também se preferir pode ser instalado em um Windows Server. 
	Vou repassar como instalar o OCS Server em um servidor Linux Debian. 


Nome do servidor:  ocs (Pode usar o nome que desejar)
IP do servidor: ex: 192.168.0.10 (Esse é o IP do seu servidor)

Com Debian 11 instalado, entrar já como root, - su e
atualizar os pacotes:

# apt update

Instalar os pacotes, serviço web, linguagem perl, php o banco mariadb.

Instalar o apache:

# apt install apache2 -y

Instalar mariadb

# apt install mariadb-server -y

Execute “mysql_secure_installation”, dê enter na primeira opção, depois vai pedir para
criar uma senha do banco:

# mysql_secure_installation
.....
Set root password? [Y/n] Y
New password: sua senha
Re-enter new password: sua senha denovo

Depois vai pedir se pode remover usuário Anônimo,
por segurança sim!

Remove anonymous users? [Y/n] Y

No resto só dá Enter até aparecer a mensagem:

Thanks for using MariaDB !

Agora criar o banco de dados, foi criado com o nome “ocsdb”

# mysql -u root -p -e "CREATE DATABASE ocsdb"

Ver o o bando criado

# mysql -u root -p -e "SHOW DATABASES"


O Banco de dados fica assim:

+------------------------+
| Database                   |
+-------------------------+
| information_schema  |
| mysql                         |
| ocsdb                          |
| performance_schema |
+-------------------------+

Criar um usuário, o nome de usuário e a senha:

# mysql -u root -p -e "CREATE USER 'usuariodb'@'localhost' IDENTIFIED BY 'senha definida por você'"

Vai perdir a senha do root do banco , só digitar e dar enter.

Depois conceda permissão(privilégios) no banco ocsdb para o usuário usuariodb

# mysql -u root -p -e "GRANT ALL PRIVILEGES on ocsdb.* TO 'usuariodb'@'localhost'"

Vai perdir denovo a senha do root do banco , só digitar e dar enter.

Banco de dados tudo ok, agora instalar o perl
e algumas extensões necessárias para comunicação com o servidor:

# apt install libxml-simple-perl libdbi-perl libdbd-mysql-perl libapache-dbi-perl libnet-ip-perl libsoap-lite-perl libarchive-zip-perl make build-essential -y

Depois executar o comando:

# cpan install XML::Entities

Dê enter para "yes".

No guia de instalação do site ocsinventory-ng.org não cita , mas tive que instalar mais 3 dependências: libswitch-perl, libmojolicious-perl e libplack-perl

# apt install libswitch-perl libmojolicious-perl libplack-perl -y

Instando PHP e algumas extensões necessárias:

# apt install php7.3-gd php-pclzip make build-essential libdbd-mysql-perl libnet-ip-perl libxml-simple-perl php php-mbstring php-soap php-mysql php-curl php-xml php-zip -y

Tudo pronto e instalado, baixar e instalar o OCS SERVER.

Baixando o Ocs Inventory a última versão (versão 2.9.2 - 08/04/2022):

Acesse o site  www.ocsinventory-ng.org,  clique no menu “OCS INVENTORY–>DOWNLOAD”.
Clique na opção “OCS Inventory Serveur Unix/Linux“
vai pedir um e-mail, colocar o e-mail que os links do instalador vai chegar na caixa de entrada,
vai ter os links do instalador do servidor quanto dos agentes. 
Depois que que fez o dowload,
Dentro da pasta onde está o arquivo “OCSNG_UNIX_SERVER-2.9.2.tar.gz” baixado, executar comando tar  para extrair os arquivos.

# tar xvf OCSNG_UNIX_SERVER-2.9.2.tar.gz

Acesse a pasta gerada e executar o comando “sh ./setup”

# ./setup.sh

Tecle enter em todas as perguntas, umas 10 pelo menos que surgirá na tela. 
Sobre banco de dados e dependências.

Depois executar o comando “sudo /usr/sbin/a2enconf ocsinventory-reports” e reinicie o apache

# /usr/sbin/a2enconf ocsinventory-reports

# systemctl restart apache2.service

Abra o navegador e acesse: 192.168.0.88/ocsreports. Preencha com os dados criados no banco de dados acima.
No campo “Servidor MySql” pode colocar tanto localhost quanto o ip da do servidor.

Usuário Mysql: usuariodb

Senha Mysql: a senha que escolheu

Servidor Mysql: localhost

Porta Mysql: 3306

Habilitar SSL: Não

Avance atá a tela de login. Use usuário admin e senha admin. Altere a senha após primeiro acesso.

acesso:

http://192.168.0.88/ocsreports

login: admin

senha: a que você escolheu, depois só alterar.



Pronto, Servidor ok, só instalar os agents nas máquinas e smartphones.


Página official do OCS: https://ocsinventory-ng.org/

Pégina de demonstração: https://demo.ocsinventory-ng.org/

user: demo
senha: demo



Everson Pruciano Contini - 08 de Abril de 2022.

https://www.linkedin.com/in/everson-pruciano-contini-243b32182/
