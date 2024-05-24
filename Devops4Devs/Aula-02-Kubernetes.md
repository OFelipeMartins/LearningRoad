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
![image](https://www.google.com/url?sa=i&url=https%3A%2F%2Fblog.devops.dev%2Fkubernetes-components-and-architecture-8bc0b1d41754&psig=AOvVaw3adgmwK9fGybMhS5hosb2z&ust=1716646501020000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCJCXjNm8poYDFQAAAAAdAAAAABAE)

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
 * Vamos criar o primerio cluster kubernetes:
   * Crie um cluster básico com o comando: `k3d cluster create`
   * Assim ele cria o cluster kubernetes com apenas um Worker Node e com nome de k3s-default. Ele configura também o kubectl e cria alguns container:
   * Liste os nós criados: `kubectl get nodes`
   * Liste os container criados para formar o cluster kubernetes: `docker ps`
     * Um para os ferramentais para gerencia dos containers.
     * Outro usando proxy para fazer o load balancer na frente do clustes. Ainda que seja apenas um nó.
     * Por fim um terceiro como Woker Node
   * Listes os cluster kubenertes criados: `k3d cluster list`<br>
     NAME          SERVERS   AGENTS   LOADBALANCER<br>
     k3s-default   1/1       0/0      true
   *  





















