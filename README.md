# Docker

Docker é um projeto de software livre para automatizar a publicação de sistemas através de contêineres. Os contêineres são autossuficientes, portáteis e podem ser executados na nuvem ou localmente. O uso do Docker pode auxiliar muito no processo de distribuição de aplicações, dispensando todo o processo de configuração de ambiente, o que agiliza a implantação.

O Docker facilita o processo de publicação de sistemas através do uso de containers. Esses containers carregam a imagem de um sistema operacional configurado previamente pelo desenvolvedor e podem conter ferramentas de desenvolvimento e até mesmo o seu próprio aplicativo. Uma imagem pode ser utilizada em diversos containers, otimizando assim o processo de implantação de software. O desenvolvedor só precisará configurar a imagem contendo sua aplicação uma vez, depois disso basta criar containers que carreguem essa imagem.

Desse modo, o Docker ajuda o desenvolvedor a ganhar tempo nas publicações, abstraindo toda a parte de configuração, e possibilitando um maior foco no desenvolvimento.

# Recursos do Docker
O Docker possui vários recursos para utilização e criação de containers, a seguir, irei listar alguns deles.
* Dockerfile
* Docker Volume
* Docker Compose
* Docker Swarm

## Dockerfile
Para executarmos os containers do Docker, precisamos de uma imagem, que nada mais é do que uma lista de comandos que nosso container irá executar quando for executado. Essa imagem pode ser obtida por download direto do Docker Hub, que é o repositório oficial de imagens Docker, criadas pela comunidade. Outra forma de se obter uma imagem, é criando ela a partir de um *Dockerfile*, que, assim como a imagem serve base para o container, ele serve de base para a imagem. No dockerfile especificamos qual a imagem base do sistema, o que será executado e tudo o que nossa imagem precisa. Para mais detalhes, acesse o [site](https://docs.docker.com/engine/reference/builder/).
### Instruções para criação de um Dockerfile 
#### FROM 
A instrução FROM é a mais utilizada para a criação de Dockerfiles, isso porque ela é obrigatória, pois é a base do Dockerfile. Com essa instrução, pode-se definir qual será o ponto de partida da imagem que criaremos com o nosso Dockerfile, ou seja, se eu quiser utilizar a imagem do Java para produzir meu container, basta que eu especifique para utilizar a imagem do openjdk como base.

#### RUN 
A instrução RUN é bem interessante. Ela pode ser executada uma ou mais vezes e, com ela, posso definir quais serão os comandos executados na etapa de criação de camadas da imagem. Essa instrução permite executarmos diversos comandos durante a criação da imagem, como atualizar repositórios, baixar alguma biblioteca, entre muitos outros. Quando utilizarmos essa imagem para a criação de um container, esses comandos não serão mais executados, pois já foram executados no momento de criação da imagem. Um ponto interessante da instrução é que cada vez que ela é utilizada no Dockerfile, gera uma camada de execução, que pode ser reutilizada em outras criações futuras, sem a necessidade de ser executada novamente. Por exemplo, se uma imagem é criada e o comando RUN é utilizado 3 vezes, porém, após a criação percebeu-se que faltou uma instrução no Dockerfile, quando essa instrução for adicionada e a imagem for gerada novamente, os comandos RUN que já haviam sido executados inicialmente não serão executados de novo, economizando tempo de execução. 

#### CMD e ENTRYPOINT 
A instrução CMD faz a mesma coisa que a instrução RUN, porém os comandos não são executados na criação da imagem, mas sim na criação do container se não for passado nenhum parâmentro na execução do container, se isso acontecer, o parâmetro sobrescreverá a instrução do CMD. Outra característica é que quando utilizamos várias instruções CMD, elas se sobrescrevem e somente a última é executada. 
A instrução ENTRYPOINT faz a mesma coisa da instrução CMD, porém sem sobrescrever, ou seja, todos os ENTRYPOINTS são executados normalmente.

#### ADD e COPY 
A instrução ADD faz a cópia de um arquivo, diretório ou até mesmo o download de uma URL de nossa máquina host e coloca dentro da imagem. Caso o arquivo passado esteja com extensão .tar, a instrução ADD já adiciona o arquivo descompactado dentro da imagem.
Já a instrução COPY faz apenas a passagem de arquivos ou diretórios do host para a imagem, sem a possibilidade de fazer download de URL.

#### EXPOSE 
O EXPOSE é responsável para indicar ao Docker quais as portas que o container estará ouvindo por conexões, devemos utilizar as portas tradicionais para as aplicações, como por exemplo em um apache, expor a porta 80, em um mysql expor a porta 3306.
A opção EXPOSE é diferente da opção -p ou --publish do comando docker container run. O EXPOSE só tem funcionalidade para o Dockerfile, uma vez que o container é criado sem publicar a porta, o docker sabe que o container estará ouvindo as portas indicadas pelo EXPOSE

#### VOLUME 
Essa instrução cria uma pasta em nosso container que será compartilhada entre o container e o host. Quando utilizamos o comando VOLUME passando uma pasta, essa pasta será criada dentro do container e todos os arquivos criados dentro do containers e armazenados nessa pasta poderão ser acessados pelo host no caminho /var/lib/docker/volumes.

#### WORKDIR
Essa instrução tem o propósito de definir o nosso ambiente de trabalho. Com ela, definimos onde as instruções CMD, RUN, ENTRYPOINT, ADD e COPY executarão suas tarefas, além de definir o diretório padrão que será aberto ao executarmos o container.

## Docker Volume
O Docker Volume é uma forma de salvar dados usados e gerados por containers e compartilhá-los entre eles. Mais informações no [site](https://docs.docker.com/storage/volumes/).
## Docker Compose
Em algumas ocasiões, para que uma aplicação funcione, precisamos subir vários containers, cada um com sua funcionalidade, para facilitar isso, existe o Docker Compose. Ele funciona como um "orquestrador" de containers, através dele, podemos gerenciar a execução de vários containers ao mesmo tempo, pelo arquivo *docker-compose.yml*. Mais informações no [site](https://docs.docker.com/compose/).
## Docker Swarm
O Swarm é a ferramenta do docker para gerenciamento de *cluster*. Através dele, podemos iniciar um cluster e partir do master e adicionar vários hosts com poucos comandos. Mais informações no [site](https://docs.docker.com/engine/swarm/).

# Instalação 
## Linux Ubuntu
Antes de instalar, é recomendável desinstalar possíveis versões antigas do Docker, utilizando o seguinte comando:
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
Para a instalação, a forma recomendada é instalar utilizando os repositórios docker. Para isso, basta seguir o passo-a-passo.

Primeiramente, atualize o repositório da máquina.
```
$ sudo apt-get update
```
A seguir, instale os pacotes necessários para o Docker:
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
Após feita a instalar os pacotes, o próximo passo é instalar a chave para poder acessar os repositórios Docker:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Por fim, use o seguinte comando para configurar o repositório estável do Docker:
```
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Este comando funciona para a arquitetura x86_64 / amd64. 
Para instalar o Docker em outras distribuições do Linux, ou instalar no ubuntu, mas via pacote DEB e instalação manual, vá para o [manual oficial](https://docs.docker.com/engine/install/) do Docker e escolha sua opção.

Após a instalação, para testar se foi instalado o Docker, rode o seguinte comando: 
```
sudo docker --version 
```
Você obterá uma saída parecida com a seguinte: 
``` 
Docker version 20.10.8, build 3967b7d
```

## Windows 
A instalação no Windows é mais simples, basta acessar o [site](https://docs.docker.com/docker-for-windows/install/) e fazer o download do executável para Windows.
Em seguinda, executar e instalar o Docker Desktop. No [site](https://docs.docker.com/docker-for-windows/install/) oficial, estão os requisitos para instalação do Docker no windows.

# BRAMS em container
## Download
Primeiramente, para poder executar o modelo BRAMS em container, temos que fazer o download da imagem que foi colocada no Docker Hub. Para isso, após ter instalado o Docker, basta rodar o seguinte comando:
```
sudo docker pull guilhermepretto/brams:brams5.6
```
Esse comando irá baixar a imagem do BRAMS para a sua máquina.
Para rodar essa imagem, é necessário o arquivo RAMSIN, o qual pode ser criado e editado através do programa BRABU, o qual possui versão em container e versão Web. A versão em container possui funcionalidades e variáveis mais avançadas, as quais, para uma primeira execução de teste, talvez não seja, necessárias. Dessa forma, aconselha-se utilizar a versão Web, encontrada no seguinte link: [BRABU](http://www.cempa.ufg.br/brabu/)

Para baixar a imagem do BRABU em container, digite o seguinte comando:
```
sudo docker pull guilhermepretto/brams:brabu3.5
```
## Execução
Para a execução, é necessário estar dentro da pasta do modelo de testes, pois o container precisa mapear as pastas datain, dataout, shared_datain e tables, além de ter o arquivo RAMSIN para o teste. Estando dentro da pasta de testes, basta rodar o comando: 
```
sudo docker run --rm -v $PWD/datain:/root/bin/datain -v $PWD/dataout:/root/bin/dataout -v $PWD/shared_datain:/root/bin/shared_datain -v $PWD/tables:/root/bin/tables --name brams guilhermepretto/brams:brams5.6 /root/run-brams -np 2   -hosts localhost:2
```
Este comando irá rodar o container do BRAMS a partir do arquivo RAMSIN base e das tabelas de exemplo. Os arquivos de saída ficam na pasta dataout.

