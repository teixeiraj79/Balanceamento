# Balanceamento
Objetivo do projeto: Criação de ambiente web com balanceamento de carga.

Para executar o projeto é necessário:
* Virtualizador: Virtual Box.
* Orquestrador: Vagrant.
* Servidores virtuais: A escolher.



# **Inicializando** 

**1° Passo:**
 1. Instalação do VirtualBox
 2. Instalação do Vagrant
 3. Definir box a ser utilizada
 4. Criar duas máquinas virtuais com Windows Server 2016 via vagrant
 
 **Comandos para criar máquinas virtuais:**
```
Vagrant init+box
Vagrant up
 ```
 
 **2° Passo:**
 
 1. Instalar o serviço web (IIS) via powershell via comandline em ambas as máquinas
 2. Acessar o http://localhost para verificar se o IIS está inicializando
 3. Modificar arquivo HTM do IIS de uma das máquinas para que ela seja diferenciada durante o balanceamento de carga
 4. Atribuir IP para as máquinas através do VagrantFile, presente na pasta onde o projeto foi iniciado. Cada máquina deve ter 1 IP public e 1 IP private.
 
 **Comando para instalar o IIS via powershell:**
 ```
install-windowsfeature -name web-server -includemanagementtools
 ```
 **Comando para atribuir IP no Vagrantfile:**
 ```
 config.vm.network "public_network", ip: "192.168.0.211"
 config.vm.network "private_network", ip: "192.168.2.11"
 ```
 **3° Passo:**
 
 1. Liberar porta 80 em ambas as máquinas Windows.
 2. Definir uma box Linux 
 3. Criar uma terceira máquina virtual com a box escolhida, que servirá para o balanceamento de carga entre as outras máquinas.
 
 **Comando para criar máquina virtual:**
```
Vagrant init+box
Vagrant up
 ```
 
 **4° Passo:**
 
 1. Inicializar máquina Linux e acessar o console da mesma
 2. Instalar Nginx na máquina
 
 **Comando para instalar Nginx na máquina Linux:**
```
sudo apt-get install nginx. 
 ```
 
 **5° Passo:**
 
 1. Entrar na máquina como Root
 2. Entrar em diretório de configuração de Nginx
 3. Comentar os includes com # para aqueles que não estão comentados dentro do arquivo de configuração
 4. Adicionar script de balanceamento de carga ao final da { de HTML
 5. Dar crtl+C para salvar as alterações
 6. Digitar o seguinte comando para reiniciar o Nginx: nginx -s reload
 
  **Comando para entrar como root na máquina:**
```
sudo su -
 ```
  **Comando para entrar em diretório de configuração do Nginx**
```
cd /etc
cd nginx
sudo nano nginx.conf
 ```
 
   **Comando para adicionar nas configurações do Nginx**
```
server{
listen 80;
location / {
proxy_pass http://app;
}
}

upstream app{
server 192.168.2.11 weight=7;
server 192.168.2.12;

}
}
 ```
 **7° Passo**
 
 1. Verificar se o balanceamento de carga está funcionando corretamente atráves da máquina Linux
 2. Digitar o IP private de uma máquina por vez em algum browser através da máquina pessoal, apenas uma máquina por vez.
 3. Se o endereço for redirecionado para a página configurada no 2° Passo, o projeto então estará concluido.
 4. Testar as duas máquinas para que não haja erro.
 
   **Comando para verificar se o balanceamento de carga está ativo em máquina Linux**
```
ping 192.168.2.11
 ```
