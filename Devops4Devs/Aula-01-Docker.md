# Introdução:<br>
Nesta aula veremos o que são Containers e como o Docker facilita nossa criação e gestão destes container.

# Objetivos:<br>
Entender e praticar criação de containers com Docker, usando um ambiente linux virtualizado com WSL e usando Docker Hub para gerenciar as imagens que criaremos de nossos containers.

# Requisitos:<br>
* Um ambiente linux.<br>
  WSL.
* [Node.js e o gerenciador de pacote NPM instalado](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl).<br>
* Docker<br>
  Instalado diretamento em sua distro WSL ou Docker Desktop instalado em seu SO.<br>
  Certifique-se de criar um arquivo .wslconfig no diretório de seu usuário contendo as seguintes declarações para limitação de CPU e Memória:<br>
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
    docker container run hello-world<br>
  * Liste os containers criados, em execução e parados:<br>
    docker container ls -a<br>
  * Remova seu container:<br>
    docker container rm <CONTAINER ID | NAME><br>

  * Container de banco de dados com imagem do postres e suas variáveis de ambiente declaradas após cada "-e":<br>
    docker container run -e POSTGRES_PASSWORD=newspwd -e POSTGRES_USER=newsuser -e POSTGRES_DN=news postgres<br>
  * Você deve reparar que seu terminal ficará travado. Para que seu terminal fique sempre disponível, devemos declarar a variável -d para que possamos rodar nossos container de forma "detach", rodando em background. Cancele o container em execução e libere seu terminal com Ctrl+C. Após liberado, executo o mesmo comando porém agora em modo detach.<br>    
    docker container run <b>-d</b> -e POSTGRES_PASSWORD=newspwd -e POSTGRES_USER=newsuser -e POSTGRES_DN=news postgres<br>
  * Acessando o terminal do container:<br>
    docker container exec -it <CONTAINER ID | NAME> /bin/bash<br>
  * Com isso é possível acessar o banco de dados via terminal, mas não é a melhor forma. Saia do terminal com o comando exit e vamos subir outro container de uma forma que possamos acessa o banco de dados por meio de uma aplicação local como DBeaver. Para isso vamos declarar uma novo parâmetro -p para definirmos as portas que usaremos de nossa máquina para a porta do container.<br>
    docker container run -d <b>-p 5432:5432</b> -e POSTGRES_PASSWORD=newspwd -e POSTGRES_USER=newsuser -e POSTGRES_DB=news postgres<br>
  * Abra o DBeaver em sua máquina, faça a conexão com o Postgres e coloque suas credenciais definidas pelas variáveis de ambiente.   
  * Tente remover seus containers em execução com o comando já utilizado, porém,, agora com o parâmetro -f para forçar o stop dos containers para que seja possível efetivamente removê-lo:<br>
    docker container rm <CONTAINER ID | NAME> <b>-f</b><br>

# Parte 3 - Clonando a aplicação do Portal de Notícias.<br>
  * Clone o projeto do portal de notícia<br>
    git clone https://github.com/KubeDev/devops4devs-01.git<br>
  * Edite se necessário o arquivo devops4devs-01/src/models/post.js com as declarações corretas das variáveis de ambiente nas linhas 3, 4 e 5.<br>
  * Acesse o diretório do scr e baixe os pacotes necessários para a aplicação:<br>
      cd devops4devs-01/src<br>
      npm install<br>
  * Ainda dentro deste diretório, rode a aplicação de forma externa, sem o uso de containers:<br>
      node server.js<br>
    * Acesse a aplicação no seu navegador:<br>
        localhost:8080<br>
        









    
