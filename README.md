# Docker
Docker - Comandos, dicas e códigos para ajudar usar o Docker

# Instalação

https://docs.docker.com/desktop/install/windows-install/

## WSL

Instalação do WSL (Windows Subsystem for Linux ou Subsistema Windows para Linux) no link(https://learn.microsoft.com/pt-br/windows/wsl/install). O WSL irá garantir um ambiente no qual o Docker irá funcionar;

- wsl -l -v (comando para listar as distros)
- wsl -d <distrubition name> (comando para logar na distro)
- wsl -s <distribution name> (comando para definir a distro padrão)
- wsl --set-version <distribution name> <versionNumber> (comando para indicar qual versão do wsl a distro deve usar Ex: wsl --set-version Ubuntu 2 )
- wsl --terminate <Distribution Name> (comando para encerra uma distro específica Ex: wsl --terminate Ubuntu)
- Link para comandos básicos do wsl (https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

# Back-end Docker Desktop WSL 2 no Windows

O Windows Subsystem for Linux (WSL) 2 é um kernel Linux completo criado pela Microsoft, que permite que as distribuições do Linux sejam executadas sem o gerenciamento de máquinas virtuais

https://docs.docker.com/desktop/wsl/

# Docker Desktop for Windows 

https://docs.docker.com/desktop/install/windows-install/

# Criar uma conta gratuita fo Docker Hub

https://hub.docker.com/signup

# Comandos (Windows Terminal)

- docker run hello-world
- docker pull <Image> (Comando para baixar uma imagem Ex: docker pull ubuntu)
- docker ps (comando para listar containers ativos)
- docker ps -a (comando para listar todos os containers ativos ou não)
- docker ps -s (comando para listar os containers ativos com o seu tamanho) 
- docker run ubuntu sleep 1d (comando para criar e rodar o container (ubuntu) e executar o comando "sleep 1d" )
> Nota: Um container só permanece ativo se houver algum processo sendo executado por ele.
- docker stop <id> ou <name> (comando que para o container usando o id ou nome do container. Use o docker ps para saber o nome ou id do containter que deseja parar.)
- docker start <id> (comando para iniciar um container específico através do id. Use docker ps para o id do container.)
- docker exec -it <id> bash (comando para executar o container em modo interativo Exemplo: docker exec -it bf5354794e80 bash)
- docker pause <id> (comando para pausar um container)
- docker unpause <id> (comando para despausar um container)
- docker rm <id> (comando para remover um container)
- docker rm <id> --force (comando para forçar parada e remover um container)
- docker run -it <Image> <command> (comando para criar e executar o container em modo interativo. Exemplo: docker run -it ubuntu bash)
- docker run -d <image> (comando para executar em background a imagem. Exemplo: docker run -d dockersamples/static-site)
- docker stop $(docker container ls -q) (comando para parar todos os containers de uma única vez o -q que representa os ids dos containers.)
- docker container rm $(docker container ls -aq) --force (comando para remover todos os containers inclusive os que estão parados por isso o -a o -q pega os ids.)

## Mapeando portas
- docker run -d -P <image> (comando para executar em background a imagem e fazer o mapeamento da porta do container para alguma porta do meu host. Exemplo: docker run -d -P dockersamples/static-site)
- docker port <id> (comando para ver em qual porta foi mapeada a porta do container para meu host)
- docker run -d -p 8080:80 <image> (comando para executar em background a imagem e fazer o mapeamento da porta(80) do container para uma porta(8080) específica do meu host. Exemplo: docker run -d -p 8080:80 dockersamples/static-site)

## Imagem
- docker images (comando para exibir as imagens que temos baixadas na máquina)
- docker inspect <id_da_imagem> (comando para ver as informações da imagem)
- docker history <id_da_imagem> (comando para ver as camadas da imagem)
- docker rmi <id_da_imagem> (comando para remover uma imagem. Exemplo: docker rmi 9b5a0697feb6)
- docker rmi $(docker image ls -aq) --force (comando para remover todas as imagens)

## Criando a primeira imagem

> Link com referência para os comandos https://docs.docker.com/engine/reference/builder/

- criar arquivo com o nome de Dockerfile (sem extensão)
- crie uma pasta exemplo-node (Exemplo: C:\Users\Diogo\Desktop\exemplo-node)
- FROM node:14 (estou dizendo para fazer uma cópia de uma imagem com o Node na versão 14)
- WORKDIR /app-node (estou defininindo o diretório padrão do projeto)
- ARG PORT_BUILD=6000 (estou definindo um argumento(variável) para poder indicar uma porta que será usada no container mas ele só funciona no tempo de criação da imagem)
- ENV PORT=$PORT_BUILD (estou definindo uma variável de ambiente que será usada dentro do da imagem)
- EXPOSE $PORT_BUILD (estou indicando qual porta está sendo usada dentro da imagem)
- COPY . . (estou copiando os arquivos da meu diretório atual para o diretório app-node definido no WORKDIR)
- RUN npm install (estou executando o comando npm install dentro da pasta do nosso projeto para poder instalar as dependências do Node. Esse comando é usado durante a criação da imagem.)
- ENTRYPOINT npm start (estou executando o projeto node dentro do diretório app-node. Esse comando é usado depois da criação da imagem )

```

FROM node:14
WORKDIR /app-node
ARG PORT_BUILD=6000
ENV PORT=$PORT_BUILD
EXPOSE $PORT_BUILD
COPY . .
RUN npm install
ENTRYPOINT npm start

```

- docker build -t <nome_da_imagem>:<version> (comando para criar a imagem Exemplo: docker build -t solutionswebdevops/app-node:1.0 )
- ERROR: "docker buildx build" requires exactly 1 argument. Tente acrescentar um '.' (ponto) ao final do comando docker build -t <nome_da_imagem>:<version> . ou
- ERROR: "docker buildx build" requires exactly 1 argument. Tente acrescentar um './' (ponto e barra) ao final do comando docker build -t <nome_da_imagem>:<version> ./
- docker run -d solutionswebdevops/app-node:1.0 (executando nossa imagem)
- docker run -d -p 8080:6000 solutionswebdevops/app-node:1.0 (executando nossa imagem e atribuindo uma porta caso não tenha feito isso no Dockerfile)

# Subindo imagem para o Docker Hub

- Link https://hub.docker.com/

- No terminal vai ser necessário autenticar seu login para poder subir alguma imagem para o Docker Hub.
- docker login -u <nome_usuario> 
- docker push <nome_da_imagem> (Exemplo: docker push solutionswebdevops/app-node:1.0)

- docker tag <imagem_copiada> <imagem_copiada_com_novo_nome> (Exemplo: docker tag solutionswebdevops/app-node:1.0 websolutions/app-node:1.0) copia uma imagem com um novo nome

# Persistindo dados no Container (Volumes, bind mounts, tmpfs)

- Link https://docs.docker.com/storage/

## Bind Mount

- docker run -it -v <path/nome_do_diretorio>:<nome_do_diretorio_no_container> <imagem>
(comando para criar um container associado a um diretório. Exemplo: docker run -it -v /home/diogo/volume-docker:/app ubuntu)
- docker run -it --mount type=<type>,source=<path/nome_do_diretorio>,target=<nome_do_diretorio_no_container> <imagem> <comando>
(Exemplo: docker run -it --mount type=bind,source=/home/diogo/volume-docker,target=/app ubuntu bash)

## Volumes

- docker volume create <nome_do_volume> (comando para criar um volume no docker. Exemplo: docker volume create meu-volume)
- docker run -it -v meu-volume:/app ubuntu bash (Exemplo de criação de container utilizando um volume)
- docker run -it --mount source=meu-volume,target=/app ubuntu bash

# Tmpfs

- docker run -it --tmpfs=/app ubuntu bash
- docker run -it --mount type=tmpfs,destination=/app ubuntu bash

# Comunicação através de redes

- docker inspect <id> (comando para obter informações do container)
- docker network ls (comando para listar as redes)
- docker network create --driver bridge <nome_da_rede> (comando para criar uma rede no docker. Exemplo: docker network create --driver minha-bridge)
- docker run -it --name ubuntu1 --network minha-bridge ubuntu bash (comando para atribui uma rede a um container que está sendo criado e nomeado por nós.)
- docker run -it --name ubuntu2 --network none ubuntu bash (comando para atribui uma rede (none - que vai isolar o container da rede) a um container que está sendo criado e nomeado por nós.)
- docker run -it --name ubuntu3 --network host ubuntu bash (comando para atribui uma rede (host - que vai ligar a rede do container ao host) a um container que está sendo criado e nomeado por nós.)

## Pequeno exemplo de comunicação entre dos containeres

Criando uma rede Bridge
- docker network create --driver minha-bridge

Baixando imagem do Mongo 4.4.6
- docker pull mongo:4.4.6

Criando container com o nome meu-mongo
- docker run -d --network minha-bridge --name meu-mongo mongo:4.4.6

Baixando imagem do aluradocker
- docker pull aluradocker/alura-books:1.0

Criando e mapeando a porta do container alurabooks para porta 3000 do meu host
- docker run -d --network minha-bridge --name alurabooks -p 3000:3000 aluradocker/alura-books:1.0

Executar no navegador http://localhost:3000 e depois http://localhost:3000/seed para carregar os livros e voltar para http://localhost:3000 e recarregar a página.


# Docker Compose (Executando múltiplos containeres)

- Criar uma pasta com o nome "ymls"
- Criar um arquivo docker-compose.yml dentro da pasta
- Depois criado o arquivo com a codificação logo abaixo iniciar comando docker-compose up
- docker-compose up (dentro da pasta onde está o arquivo docker-compose.yml)
- docker-compose ps
- docker-compose down

```

version: '3.9'
services:
  mongodb:
    image: mongo:4.4.6
    container_name: meu-mongo
    networks:
      - compose-bridge

  alurabooks:
    image: aluradocker/alura-books:1.0
    container_name: alurabook
    networks:
      - compose-bridge
    ports:
      - 3000:3000
    depends_on:
      - mongodb

networks:
  compose-bridge:
    driver: bridge

```

# Comandos (Linux)

- sudo su (permite usar o terminal no modo super usuário)
- apt-get update (atualiza a imagem do ubuntu)
- apt-get install iputils-ping -y (programa que ajuda a fazer "ping" na rede)

# Exemplo de container Mysql com algumas configurações

- docker run --name container_mysql --network network_bridge -v volume_mysql:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest
- docker run --name container_mysql_5743 --network network_bridge -v volume_mysql_5743:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.43
- docker run --name container_mysql_8034 --network network_bridge -v volume_mysql_8034:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8.0.34

# Links
https://learn.microsoft.com/pt-br/windows/wsl/install

https://docs.docker.com/desktop/wsl/

https://laravel.com/docs/10.x/installation

https://laravel.com/docs/10.x/sail

https://learn.microsoft.com/pt-br/windows/wsl/setup/environment#set-up-your-linux-user-info

https://learn.microsoft.com/pt-br/windows/wsl/install
