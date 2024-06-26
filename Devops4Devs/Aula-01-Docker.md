# Introdução:<br>
Nesta aula veremos o que são Containers e como o Docker facilita nossa criação e gestão destes container.

# Objetivos:<br>
Entender e praticar criação de containers com Docker, usando um ambiente linux virtualizado com WSL e usando Docker Hub para gerenciar as imagens que criaremos de nossos containers.

# Requisitos:<br>
* Um ambiente linux.<br>
  [WSL](https://www.youtube.com/watch?v=2X61S4mf6is&pp=ygUVZmFicsOtY2lvIHZlcm9uZXogd3Ns).
* [Node.js e o gerenciador de pacote NPM instalado](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl).<br>
* Docker<br>
  [Instalado diretamento em sua distro WSL ou Docker Desktop instalado em seu SO](https://www.youtube.com/live/2X61S4mf6is?si=W-_caH2scFlPAETO&t=4392).<br>
  Certifique-se de [criar um arquivo .wslconfig](https://www.youtube.com/live/O813vtoaXmc?si=NNkHa2n44eA20Itb&t=5427) no diretório de seu usuário contendo as seguintes declarações para limitação de CPU e Memória:<br>
  \# Settings apply across all Linnux distros running on WSL 2<br>
  [wsl2]
  
  \# Limits VM memory to use no more than 5GB, this can be set as whole number using GB or MB<br>
  memory=5GB
  
  \# Sets de VM to use 4 virtual processors<br>
  processors=4

# Parte 1 - Uma breve descrição de Docker:<br>
  Docker é uma ferramenta de gerenciamento de container criados a partir de imagens padrão ou personalizadas.<br> 
  Assim como Docker existem outras opções para gestão de containers.<br>
  A arquitetura do Docker é composta de 3 elementos:<br>
  ![image](https://github.com/OFelipeMartins/LearningRoad/assets/57650447/ac224440-8a93-4b20-b9e2-f351035751a4)

# Parte 2 - Primeiros comandos:<br>
  * Crie seu primeiro container com a imagem hello-world:<br>
    `docker container run hello-world`
  * Liste os containers criados, em execução e parados:<br>
    `docker container ls -a`
  * Remova seu container:<br>
    `docker container rm <CONTAINER ID | NAME>`

  * Container de banco de dados com imagem do postgres e suas variáveis de ambiente declaradas após cada "-e":<br>
    `docker container run -e POSTGRES_PASSWORD=newspwd -e POSTGRES_USER=newsuser -e POSTGRES_DN=news postgres`
  * Você deve reparar que seu terminal ficará travado. Para que seu terminal fique sempre disponível, devemos declarar a variável -d para que possamos rodar nossos container de forma "detach", rodando em background. Cancele o container em execução e libere seu terminal com Ctrl+C. Após liberado, executo o mesmo comando porém agora em modo detach (-d).<br>    
    `docker container run -d -e POSTGRES_PASSWORD=newspwd -e POSTGRES_USER=newsuser -e POSTGRES_DN=news postgres`
  * Acessando o terminal do container:<br>
    `docker container exec -it <CONTAINER ID | NAME> /bin/bash`
  * Com isso é possível acessar o banco de dados via terminal, mas não é a melhor forma. Saia do terminal com o comando exit e vamos subir outro container de uma forma que possamos acessa o banco de dados por meio de uma aplicação local como DBeaver. Para isso vamos declarar uma novo parâmetro -p para definirmos as portas que usaremos de nossa máquina para a porta do container(-p 5432:5432).<br>
    `docker container run -d -p 5432:5432 -e POSTGRES_PASSWORD=newspwd -e POSTGRES_USER=newsuser -e POSTGRES_DB=news postgres`
  * Abra o DBeaver em sua máquina, faça a conexão com o Postgres e coloque suas credenciais definidas pelas variáveis de ambiente.   
  * Tente remover seus containers em execução com o comando já utilizado, porém, agora com o parâmetro -f para forçar o stop dos containers para que seja possível efetivamente removê-lo:<br>
    `docker container rm <CONTAINER ID | NAME> -f`

# Parte 3 - Clonando a aplicação do Portal de Notícias.<br>
  * Clone o projeto do portal de notícias.<br>
    `git clone https://github.com/KubeDev/devops4devs-01.git`
  * Se preferir, acesse o Visual Studio Code para visualizar e editar os arquivos do projeto.
    `code .`
  * Edite se preferir o arquivo devops4devs-01/src/models/post.js com as credenciais das variáveis de ambiente nas linhas 3, 4 e 5.<br>
  * Acesse o diretório do scr e baixe os pacotes necessários para a aplicação:<br>
      `cd devops4devs-01/src`
      `npm install`
  * Ainda dentro deste diretório, rode a aplicação de forma local, sem o uso de containers:<br>
      `node server.js`
  * Acesse a aplicação no seu navegador:<br>
        `localhost:8080`
    * Preencha o novo Post e salve como exemplo
    * Se você ainda estiver com o banco de dados rodando em container, confira dentro de Banco de dados > News > Schemme > public > Tabelas > Post > Botão direito > Ver dados
      * Confira o registro do seu post
    * Este foi um exemplo de uma aplicação rodando localmento usando um banco de dados rodando em container.
# Parte 4 - Entendendo a criação de imagens.
  * Toda vez que criamos um container, utilizamos de uma imagem como base que contém todos os arquivos necessários para executar nosso processo de forma isolada no container. Então uma imagem, nada mais é, do que um file system para criação do container.
  * Para criármos uma imagem de container temos duas abordagens:
    * Docker Commit (Pior formar)
    * Dockerfile (Melhor que a anterior e a que usaremos)
      * Dockerfile é um arquivo que criamos junto com o projeto, uma receita de bolo para criação do container, com todas as intruções e passo a passo para criármos a imagem da melhor forma possível.
      * Criaremos uma imagem docker de uma aplicação mais simplificada com uso do dockerfile.
      * Baixe o repositório da aplicação Conversão de tempertatura:<br>
        `git clone https://github.com/KubeDev/conversao-temperatura`<br>
      * Uma aplicação bem simples, escrita em node.js, utilizando as prinipais bibliotecas que também utilzaremos na aplicação final desta maratona, então serve perfeitamente como exemplo para este propósito desta Parte 4 para criação de imagem. É recomandado que se faça um fork no projeto.
      * Usaremos novamente o gerenciador de pacotes do node.js, o npm, para baixar as bibliotecas que vamos utilizar executar nosso projeto como por exemplo express, body-parser, ejs...
      * Dentro do diretório conversao-temperatura/src, onde tem o arquivo package.json, rode:<br>
        `npm install`
      * Depois de concluído, você pode perceber um novo diretório, node_modules, com todas as bibliotecas utilizadas no projeto. Dentro deste existe uma grande quantidade de bibliotecas que se fazem necessárias por outras bibliotecas tbm dentro deste diretório.
      * execute o projeto:<br>
        `node server.js`
      * Acesse em seu navegador a aplicação:<br>
        `localhost:8080`
      * Faça os testes de conversão de temperatura.
  * Vamos fazer agora a contrução da imagem desta aplicação usando o Dockerfile.
    * Crie o arquivo Dockerfile. Nele iremos declarar tudo que é necessário para criármos uma imagem a partir dele posteriomente. Todas as decarações, linha a linha do arquivo final do Dockerfile virá seguinte às explicações. 
      * Primeiro vamos definir a imagem base que usaremos para criação posterior da imagem(Exato, uma imagem que compoê outra imagem). Como nossa aplicação é escrita em node.js e nós vamos precisar do node.js rodando no container, faz sentidor termos uma imagem dele declarado em nosso Dockefile. Para saber se já existe uma imagem do node, pesquisamos por "node" no Docker hub(principal repositório de imagens docker). Já no primeiro resultado podemos achar a imagem oficil do node, assim, declaramos em nosso Dockerfile `FROM node`.
      * Em seguida vamos declarar nosso diretório de trabalho dentro do container, onde colocaremos todos os arquivos do projeto. Para isso, usamos o instrução `WORKDIR /app` (Pode ser qualquer diretório, desde que seja definido previamente.
      * Em seguida vamos configurar nosso ambiente, da mesma forma que fizemos para rodar localmente, baixar os arquivo e as bibliotecas para rodar nossa apicação. Assim vamos primeiro copiar os arquivos que já temos em nossa máquina para a imagem com a instrução `COPY package*.json .` que copia tudo que inicie com package e termine com .json para o diretório atual `.`, ou seja, o /app definido no passo anterior.
      * em seguida vamos será preciso executar o comando npm install usando da instrução RUN, ficando da seguinte forma `RUN npm install`, que irá cuidar de baixar todas bibliotecas dentro do container quando
      * Vamos copiar também todo o resto dos arquivos dentro do noso diretório para a imagem com `COPY . .`.
      * Depois disso, vamos acrescentar também que seja executado o comando node server.js com a instrução `CMD ["node", "server.js"]`
      * Por último, vamos declarar um exposição da porta 8080, que é por onde iremos acessar a aplicação.
      * Resumindo, nosso Dockerfile vai ficar da seguinte forma:
        ```
        FROM node
        WORKDIR /app
        COPY package*.json .
        RUN npm install
        COPY . .
        CMD ["node", "server.js"]
        EXPOSE 8080
        ```
        * Esses foram apenas alguns dos comandos e declarações possíveis para se escolher na hora de criar um Dockerfile. Para mais opções, confira [aqui](https://docs.docker.com/reference/dockerfile/)
        * Agora para construir a imagem, usamos o seguinte comando dentro do diretório src do projeto:<br>
          `docker build -t conversao-temperatura .`
        * Confira a sua nova imagem criada e armazenada em sua máquina com o comando `docker image ls`
        * Rode seu container usando a imagem que vc acabou de criar: `docker container run -d -p 8080:8080 conversao-temperatura`
        * Acesse a aplicação pelo seu navegador: `localhost:8080`
       
        * Boas práticas para declaração da imagem base(ex: node) no seu arquivo Dockerfile:
          * Declare a versão da imagem que funciona para seu projeto. Imagens oficiais quando declaradas no arquivo Dockerfile, são sempre as mais recentes e isso pode interferir no restante da sua aplicação final. Para este caso, usaremos e declararemos como `FROM node:20.11.01`
        * Docker ignore (.dockerignore) é um arquivo de texto onde se declara tudo que vc não quer que seja copiado para a imagem final. Para este caso não copiaremos o diretório node_modules/, uma vez que este já é criado com o comando `RUN npm install`
        * Com isso, recrie sua imagem e já envie novamente para o repositório com o comando:
         `docker build -t Seu_namescpace/conversao-temperatura:v1 --push .`
# Parte 5 - Docker registry
  * Docker registry é um repositório onde guardaremos nossas imagens para que fique acessível a outras pessoas como membros da sua equipe e não fique apenas de forma local em sua máquina. Usaremos o Docker Hub para armazenar nossas imagens. Nele é possível cadastrar gratuitamente seus repositório, porém, só de forma pública.
  * Se cadastre no [Docker Hub](https://hub.docker.com/).
  * Acesse o seu repositório dentro do Docker Hub e Create repository. Defina um nome e "Create".
  * Para enviarmos nossa imagem para este repositório, precisamos primeiro colocar o nome dela no formato padrão aceito pelo Docker hub:
    * `Seu_Namespace/Nome_Do_Seu_Repositório:tag`
  * Para editar o nome da nossa imagem:
    * `docker tag conversao-temperatura Seu_Namespace/conversao-temperatura:v1`
    * Confira a alteração do nome com o comando: `docker image ls`
  * Autentique-se com seu usuário do docker hub no seu ambiente linux com o comando: `docker login`
  * Envie sua imagem para seu repositório no Docker Hub: `docker push seu_namespace/conversao-temperatura:v1`
    * É uma boa prática você manter uma imagem com a tag latest, representando a última versão do seu projeto.
    * Edite novamente o nome da imagem com a tag :latest e suba para seu repositório.





