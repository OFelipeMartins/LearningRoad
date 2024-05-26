# Introdução:<br>
Nesta aula veremos o que são Clustes e como o Kubernetes facilita nossa criação e gestão e orquestração de containers.

# Objetivos:<br>
Entender e praticar criação de clusters, criando um ambiente escalável com K3D para orquestrar nosso ambiente de containers.

# Requisitos:<br>
* Instalar K3D .<br>
  [K3D](https://k3d.io/v5.6.3/).
* [Kubectl]([https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl](https://kubernetes.io/pt-br/docs/tasks/tools/)).<br>

# Parte 1 - Uma breve descrição de Clustes em Kubernetes:<br>
 * O Kubernetes(K8s) é uma plataforma de orquestração de contêineres open-source que automatiza a implantação, o dimensionamento e o gerenciamento de aplicativos em contêineres. Imagine-o como um maestro de orquestra para seus contêineres, garantindo que eles funcionem em conjunto de forma harmoniosa e eficiente.
 * Já um Cluster Kubernetes é um grupo de máquinas virtuais ou servidores físicos que trabalham juntos para executar aplicativos em contêineres. O Kubernetes automatiza o gerenciamento desses clusters, tornando mais fácil implantar, dimensionar e gerenciar seus aplicativos.
 * A arquitetura do Kubernetes é composta da seguinte forma:
![image](https://www.opsramp.com/wp-content/uploads/2022/07/Kubernetes-Architecture-1024x648.png)

# Parte 2 - Criando um cluster kubernetes.
 * Formas e suas opções de ferramentas para criação:
   * On-Premisse:
     * Kubean, K3S, MicroK8S ...
     * Vantagens/Desvangatens: Todos os componentes do cluster kubernetes são gerenciados por você
   * Kubernetes as a Service por meios dos Cloud providers:
     * EKS, AKS, GKE...
     * Vantagem: O gerenciamento do cluster ficar por conta do Cloud Provider.
     * Desvantagem: Custo.
   * Local:
     * Minikube, K3S, MicroK8S, `kIND, k3D` (Utilizam container docker para executar a criação do ambiente local.
     * Vantagem: Ambientes Ready to go para criação de POCS
     * Desvantagem: Não serve para ambiente produtivo.
 * Vamos criar o primerio cluster kubernetes:
   * Crie um cluster básico com o comando: `k3d cluster create`
   * Assim ele cria o cluster kubernetes com apenas um Worker Node e com nome de k3s-default. Ele configura também o kubectl e cria alguns container:
   * Liste os nós criados: `kubectl get nodes`
   * Liste os container criados para formar o cluster kubernetes: `docker ps`
     * Um para os ferramentais para gerencia dos containers.
     * Outro usando proxy para fazer o load balancer na frente do clustes. Ainda que seja apenas um nó.
     * Por fim um terceiro como Woker Node
   * Listes os cluster kubenertes criados: `k3d cluster list`<br>
     ```
     NAME          SERVERS   AGENTS   LOADBALANCER<br>
     k3s-default   1/1       0/0      true
     ```
   * Delete o seu cluster: `k3d cluster delete`
     * Para o cluter default não é necessário declarar o nome para deleção. 
   *  Crie um cluster com um nome que você escolheu, com mais agents e mais servers(Contro planes):
     `k3d cluster create meucluster --servers 3 --agents 3`
   * Liste os containers que formam o cluster: `docker ps`
   * Liste seu cluster, veja e confirme quantidade de servers e agents: k3d cluster list
   * Delete seu cluster, dessa vez passando o nome: `k3d cluster delete meucluster` 
# Parte 3 - Criando um deploy de sua aplicação em ambiente kubernetes.
 * Quais elementos são criados no kubernetes e qual a função de cada um deles:
   * Pod:
     * É o menor objeto em um cluster kubernetes e é dentro deles que executamos nossos containers. Dentro deles podemos executar um ou mais containers. Eles dividem o mesmo endereço IP e, podem também, compartilhar sistemas de arquivos. Porém não é interessante se colocar todos seus elementos dentro de um mesmo POD pois, quando, falamos de repicar um ambiente, não estamos falando de replicar um container, mas sim um POD. Assim, criamos um pod para cada container. Usamos um POD para rodas mais de um container quando, por exemplo, temos um serviço como de coleta de logs para rodas paralelamente ao container principal do seu POD. 
   * Labels e Selectors
   * ReplicaSet
     * Cuida da execução dos Pods, garantindo que a quantidade específicas de réplicas de um determinado pod é a que vai estar em execução no ambiente.
   * Deployment
     * Gerencia os Replicaset, em caso de alteração do código, ele cria um novo eplicaset que por consequência irá criar o novo ambiente de pods com os containers rodando a versão atualizada da aplicação.
     * ![image](https://miro.medium.com/v2/resize:fit:1400/1*q5BhhIKnBqQqsngJG6EtQw.png)




















