# ProCRM

Este projeto visa ser um fork do projeto da Plataforma ERP Odoo (https://github.com/odoo/odoo) customizado para atender as necessidades internas de CRM da Companhia de Tecnologia da Informação do Estado de Minas Gerais - Prodemge (www.prodemge.gov.br).

Instalação do Ambiente
----------------------

Tomando como base uma VM Ubuntu Server 22.04.4 LTS seguimos os seguintes passos para preparação:

Instalação do Docker
--------------------

1. Remoção de versões antigas:
  ```
  $ for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
  ```
Para uma instalação limpa pode ser necessária a exclusão de todo conteúdo existente na pasta `/var/lib/docker`.

2. Configurar o repositório `apt` do Docker:

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```  

> NOTA
> 
> Caso utilize uma distro devivada do Ubuntu, como por exemplo o Linux Mint, será necessário utilizar `UBUNTU_CODENAME` no lugar de `VERSION_CODENAME`.

3. Instalação dos pacotes do Docker:
```
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Instalação do Portainer Community Edition:
------------------------

1. Primeiro vamos criar o volume que o Portainer Server utilizará para armazenar sua database:

```
$ docker volume create portainer_data
```

2. Baixando e instalando o Portainer:
>NOTA
>
>Desabilite o SELinux no servidor que está rodando o Docker. Caso contrário se necessário passar o argumento `--privileged` para o Docker quando fizer o deploy do Portainer.
>
>Caso for utilizar certificado SSL você deve passar a porta 9443 como parâmetro `-p 9443:9443` no lugar da porta 9000.

```
docker run -d -p 8000:8000 -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

3. Primeiro acesso ao Portainer:

   Acesse pelo navegador a interface `http://localhost:9000` e cadastre o usuário de administração do Portainer.

   
