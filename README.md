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

- docker run hello-word
- docker pull <Image> (Comando para baixar uma imagem Ex: docker pull ubuntu)
- docker ps (comando para listar containers ativos)
- docker ps -a (comando para listar todos os containers ativos ou não)
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

## Mapeando portas
- docker run -d -P <image> (comando para executar em background a imagem e fazer o mapeamento da porta do container para alguma porta do meu host. Exemplo: docker run -d -P dockersamples/static-site)
- docker port <id> (comando para ver em qual porta foi mapeada a porta do container para meu host)
- docker run -d -p 8080:80 <image> (comando para executar em background a imagem e fazer o mapeamento da porta(80) do container para uma porta(8080) específica do meu host. Exemplo: docker run -d -p 8080:80 dockersamples/static-site)

## Imagem
- docker images (comando para exibir as imagens que temos baixadas na máquina)
- docker inspect <id_da_imagem> (comando para ver as informações da imagem)
- docker history <id_da_imagem> (comando para ver as camadas da imagem)
- docker rmi <id_da_imagem> (comando para remover uma imagem. Exemplo: docker rmi 9b5a0697feb6)

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

# Comandos (Linux)

- sudo su (permite usar o terminal no modo super usuário)



# Links
https://learn.microsoft.com/pt-br/windows/wsl/install

https://docs.docker.com/desktop/wsl/

https://laravel.com/docs/10.x/installation

https://laravel.com/docs/10.x/sail

https://learn.microsoft.com/pt-br/windows/wsl/setup/environment#set-up-your-linux-user-info

https://learn.microsoft.com/pt-br/windows/wsl/install
