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
### Instruções para criação de um container 
#### FROM 
A instrução FROM é a mais utilizada para a criação de Dockerfiles, isso porque ela é obrigatória, pois é a base do Dockerfile. Com essa instrução, pode-se definir qual será o ponto de partida da imagem que criaremos com o nosso Dockerfile, ou seja, se eu quiser utilizar a imagem do Java para produzir meu container, basta que eu especifique para utilizar a imagem do openjdk como base.

#### RUN 
A instrução RUN é bem interessante. Ela pode ser executada uma ou mais vezes e, com ela, posso definir quais serão os comandos executados na etapa de criação de camadas da imagem. Essa instrução permite executarmos diversos comandos durante a criação da imagem, como atualizar repositórios, baixar alguma biblioteca, entre muitos outros. Quando utilizarmos essa imagem para a criação de um container, esses comandos não serão mais executados, pois já foram executados no momento de criação da imagem. Um ponto interessante da instrução é que cada vez que ela é utilizada no Dockerfile, gera uma camada de execução que, podem ser reutilizadas em outras criações futuras, sem a necessidade de serem executas novamente. Por exemplo, se uma imagem é criada e o comando RUN é utilizado 3 vezes, porém, após a criação percebeu-se que faltou uma instrução no Dockerfile, quando essa instrução for adicionada e a imagem for gerada novamente, os comandos RUN que já haviam sido executados inicialmente não serão executados de novo, economizando tempo de execução.

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

## Windows 
A instalação no Windows é mais simples, basta acessar o [site](https://docs.docker.com/docker-for-windows/install/) e fazer o download do executável para Windows.
Em seguinda, executar e instalar o Docker Desktop. No [site](https://docs.docker.com/docker-for-windows/install/) oficial, estão os requisitos para instalação do Docker no windows.


