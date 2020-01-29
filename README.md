## GetUp Cloud Docs

Esse reposítório foi criado para servir de complementação à documetação dos projetos do Desafio da GetUp Cloud, que são os repositórios abaixo:

[Vote](https://github.com/hebersonaguiar/vote)

[Worker](https://github.com/hebersonaguiar/worker)

[Result](https://github.com/hebersonaguiar/result)


## A proposta
A proposta desse projeto é de em base ao repositório [Example Voting App](https://github.com/dockersamples/example-voting-app) que foi repassado pela GetUp Cloud, criar um cluster Kubernetes ser que seja um serviço gerenciado.

Para esse desafio foram utilizados as seguintes tecnologias:

[Docker](https://github.com/hebersonaguiar/getupclouddocs#docker)

[Google Cloud Plataform](https://github.com/hebersonaguiar/getupclouddocs#google-cloud-plataform)

[HAproxy](https://github.com/hebersonaguiar/getupclouddocs#haproxy)

[Kubernetes](https://github.com/hebersonaguiar/getupclouddocs#kubernetes)

[Voting App](https://github.com/hebersonaguiar/getupclouddocs#voting-app)

[Helm Chart](https://github.com/hebersonaguiar/getupclouddocs#helm-chart)

[GitHub](https://github.com/hebersonaguiar/getupclouddocs#github)

[Prometheus](https://github.com/hebersonaguiar/getupclouddocs#prometheus)

[Grafana](https://github.com/hebersonaguiar/getupclouddocs#grafana)

[Elastic Stack](https://github.com/hebersonaguiar/getupclouddocs#elastic-stack)

[Nginx](https://github.com/hebersonaguiar/getupclouddocs#nginx)


## DNS e acessos desse projeto:

Aplicações

[vote.hebersonaguiar.me](https://vote.hebersonaguiar.me)

[result.hebersonaguiar.me](https://result.hebersonaguiar.me)

[grafana.hebersonaguiar.me](https://grafana.hebersonaguiar.me)

[kibana.hebersonaguiar.me](https://kibana.hebersonaguiar.me)

[prometheus.hebersonaguiar.me](https://prometheus.hebersonaguiar.me)

[alertmanager.hebersonaguiar.me](https://alertmanager.hebersonaguiar.me)

Servidores

HAproxy/Nginx -- haproxy.hebersonaguiar.me 

Master 1 -- tom.hebersonaguiar.me

Master 2 -- jerry.hebersonaguiar.me

Worker 1 -- popeye.hebersonaguiar.me

Worker 2 -- pernalonga.hebersonaguiar.me

Worker 3 -- patolino.hebersonaguiar.me

Worker 4 -- charliebrown.hebersonaguiar.me



Credenciais de acesso:

Grafana:

`Usuário: admin`

`Senha: yXRXQ3RFb143zGf7bq4N2v6phlfI2rUKjsKo2X3V`

Google Cloud:

`Usuário: hebersonaguiar.ti@gmail.com`

`Senha: 0pAP6Edu87HRD1XLpZ`

Vote, Result, Prometheus e Kibana possuem acesso livre. 

Para autenticar nos servidores é necessário utilizar chaves para isso utilize essa chave:

[Chave SSH](https://github.com/hebersonaguiar/getupclouddocs/blob/master/files/keys/id_rsa)

Para autenticar utilizando terminal linux utilize comando abaixo:

Usuário Admin `hebersonaguiar_ti`

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti <nome-do-servidor>
```

Usuário Desenvolvedor `desenvolvedor`

```bash
ssh -i <path-cahve_ssh/id_rsa> -l desenvolvedor <nome-do-servidor>
```

## Docker
O Docker é uma plataforma para desenvolvedores e administradores de sistemas para desenvolver, enviar e executar aplicativos. O Docker permite montar aplicativos rapidamente a partir de componentes e elimina o atrito que pode ocorrer no envio do código. O Docker permite que seu código seja testado e implantado na produção o mais rápido possível. 

## Google Cloud Plataform
Google Cloud Platform também conhecida como GCP é uma suíte de cloud oferecida pelo Google, funcionando na mesma infraestrutura que a empresa usa para seus produtos dirigidos aos usuários, dentre eles o Buscador Google e o Youtube.
Para esse projeto foi utilizada criado uma conta no qual foi informada anteriomente e o [Console](https://console.cloud.google.com/) pode ser acessado livremente para verificação dos recursos utilizados, entre eles Compute Engine, CLoud DNS etre outros. Os principais produtos que iremos utilizar são 

* [Cloud DNS](https://github.com/hebersonaguiar/getupclouddocs#google-cloud-plataform), onde foi utilizado o domínio `hebersonaguiar.me` apontando os momes das aplicações para os serviços de entrada de requisição, e também os DNS dos servidores.

* [Compute Engine](https://github.com/hebersonaguiar/getupclouddocs#google-cloud-plataform), onde foi criado as intâncias de vm do cluster composto por dois master, 4 workers e 1 haproxy e nginx. A criação das instâncias foram feitas manualmente, devido a divisão do cluster em relação ao tempo, ou seja, para criação utilizando como por exemplo `terraform` iria levar mais tempo para criar os arquivos e configurações do que criar manualmente pois as instâncias foram dividias em zonas e tipo.

* [Kubernetes](https://github.com/hebersonaguiar/getupclouddocs#kubernetes), utilizado para criar o cluster do Kubernetes. Nesse projeto foi criado um cluster multi-master para que possa garantir a disponibilidade do cluster caso um dos master pare de funcionar. Foi também utilizado o `kubeadm` para criação do cluster, as informações de criação de configuração do cluster estão na seção [Kubernetes](https://github.com/hebersonaguiar/getupclouddocs#kubernetes)


Criação das Instâncias:

As instâncias foram criadas com as seguintes configurações:

Hosts `tom`, `jerry`, `haproxy`, `charliebrown` foram criadas na zona `us-central-a` com 2 vCPUs, 7,5 GB de memória (n1-standard-2), disco padrão 10GB, S.O. Debian 9;

Host `pernalonga`, foi criada na zona `us-west1-a` com 2 vCPUs, 7,5 GB de memória (n1-standard-2), disco padrão 10GB, S.O. Debian 9;

Host `popeye`, foi criada na zona `us-east1-a` com 2 vCPUs, 7,5 GB de memória (n1-standard-2), disco padrão 10GB, S.O. Debian 9;

Host `patolino`, foi criada na zona `us-west2-c` com 1 vCPUs, 3,75 GB de memória (n1-standard-1), disco padrão 10GB, S.O. Debian 9;

## HAproxy

Para criação do cluster kubernetes do tipo multi-cluster é necessário algum tipo de load balancer, seguindo a documentação oficial é possível utilizar um Load Balancer de algum cloud provider como GCP ou AWS ou um HAproxy, nesse projeto iremos utilizar o HAproxy, para isso segue abaixo o passo-a-passo as configurações:

Acesso ao servidor `haproxy.hebersonaguiar.me`

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti haproxy.hebersonaguiar.me
```

Instalação do HAproxy
```bash
sudo -i
apt-get update
apt-get install -y haproxy
```

Configuração do HAproxy
```bash
cat > /etc/haproxy/haproxy.cfg <<EOF
global
user haproxy
group haproxy

defaults
mode http
log global
retries 2
timeout connect 3000ms
timeout server 5000ms
timeout client 5000ms

frontend kubernetes
bind 10.128.0.13:6443
option tcplog
mode tcp
default_backend kubernetes-master-nodes

backend kubernetes-master-nodes
mode tcp
balance roundrobin
option tcp-check
server k8s-master-0 10.128.0.42:6443 check fall 3 rise 2
server k8s-master-1 10.128.0.43:6443 check fall 3 rise 2
EOF
```
Os ip's `10.128.0.42` e `10.128.0.43` são os ips internos dos servidores masters `tom` e `jerry` respectivamente, a porta `6443` é a porta do `kube-apiserver` responsável por fornecer os serviços de comunicação do cluster, como por exemplo criar, consulta, deletar um recurso do cluster.

Após realizar as configurações acima bastar reiniciar o serviço do haproxy:

```bash
systemctl restart haproxy
```


## Kubernetes

Kubernetes ou como é conhecido também K8s é um produto Open Source utilizado para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêiner no qual agrupa contêineres que compõem uma aplicação em unidades lógicas para facilitar o gerenciamento e a descoberta de serviço

Nesse projeto foi utilizado o kubernetes na versão stable `v1.17.2`, o cluster foi criado na GCP, o Kubernetes não é voltado apenas para aplicativos sem estado. Com ele, é possível incluir armazenamento permanente e até mesmo executar um banco de dados no seu cluster.

O cluster criado contém dois masters e quatro workes, como mostra a imagem abaixo:

![nodes](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/nodes.PNG)

Para criação do cluster foram utilizados as seguintes configurações:

Configuração de sistema operacional, docker, kernel, instalação de pacotes:

Acesse aos servidores

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti <nome-do-servidor>
```

Configuração de módulos de kernel

```bash
cat > /etc/modules-load.d/k8s.conf <<EOF
br_netfilter
ip_vs_rr
ip_vs_wrr
ip_vs_sh
nf_conntrack_ipv4
ip_vs
EOF
```

Atualização e instalação de pacotes docker e kubernetes

```bash
bash <<EOF
# Lembre-se de executar nos outros masters!
apt-get update
apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common vim
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo 'deb https://apt.kubernetes.io/ kubernetes-xenial main' > /etc/apt/sources.list.d/kubernetes.list
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
EOF
```

Configuração de `cgroup-driver` (recomendação kubernetes) e log-driver

```bash
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "journald"
}
EOF
```

Após as configurações é necessário a reinicializaão dos servidores para que os módulos de kernel seja habilitados e as alterações tenham efeitos:

```bash
reboot
```

Instalação dos servidores masters

Acesse um dos servidores desiguinados como master, nesse casso foi utilizado incialmente o `tom.hebersonaguiar.me`:

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti tom.hebersonaguiar.me
```

Para iniciar o cluster através do kubeadm é preciso criar um arquivo que indicará onde está o load balancer bem como qual o endereço de rede utilizaremos tanto para a máquina como para os containers, segue abaixo:

```bash
cat > /root/kubeadm-config.yml <<EOF
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: stable
controlPlaneEndpoint: "10.128.0.13:6443" # servidor e porta do load balancer (haproxy)
networking:
  podSubnet: "10.244.0.0/16"
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: "10.128.0.42" # servidor master inicial
  bindPort: 6443
EOF
```

Realizados as configurações acima, vamos iniciar o cluster:

```bash
kubeadm init --config '/root/kubeadm-config.yml' --upload-certs
```

Ao executar o comando acima serão realizados o download dos containers necessários para funcionamento do cluster como por exemplo o `Core DNS`, `etcd` e serão realizados todas as configurações de arquivos, chaves, kubelet, etc e o resultado é o abaixo:

```bash
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 10.128.0.13:6443 --token 2wkrhv.pcdpe7qoc3z9e3j2 \
    --discovery-token-ca-cert-hash sha256:40a70e4054a6670bfc22bc7b975e6ec1a0e0a9b749c8d14a5df251f18f30509b \
    --control-plane --certificate-key b94cd6eb79955a7fde00a4d91fab2a54f8bddee8dbcad14cd0b60ceb8bb292d6

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.128.0.13:6443 --token 2wkrhv.pcdpe7qoc3z9e3j2 \
    --discovery-token-ca-cert-hash sha256:40a70e4054a6670bfc22bc7b975e6ec1a0e0a9b749c8d14a5df251f18f30509b
```

Iniciado o cluster é necessário configurar o arquivo de conexão ao cluster:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Nesse momeneto o master ainda não está configurado por completo, para finalizar é necessário a configurar a rede do cluster onde todos os nós e seus recursos se comunicarão, para isso iremos utilizar o `https://cloud.weave.works`:

```bash
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

Pronto, o primeiro master está pronto, para incluir o segundo master no cluster iremos acessar o segundo servidor designado como master:

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti jerry.hebersonaguiar.me
```

Insira o servidor no cluster com os dados informados na inicialização do cluster:

```bash
kubeadm join 10.128.0.13:6443 --token 2wkrhv.pcdpe7qoc3z9e3j2 \
    --discovery-token-ca-cert-hash sha256:40a70e4054a6670bfc22bc7b975e6ec1a0e0a9b749c8d14a5df251f18f30509b \
    --control-plane --certificate-key b94cd6eb79955a7fde00a4d91fab2a54f8bddee8dbcad14cd0b60ceb8bb292d6
```

Configure o arquivo de conexão ao cluster:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Para verificar a inclusão do cluster execute o comando abaixo e você verá dois servidores com a role `master` e o status `Ready`, agora temos um cluster inicial com dois masters.

```bash
kubectl get nodes
```

Para incluir os workers acesse todos os servidores desiguinados como workers:

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti <nome-do-servidor-worker>
```

Insira o servidor no cluster com os dados informados na inicialização do cluster:

```bash
kubeadm join 10.128.0.13:6443 --token 2wkrhv.pcdpe7qoc3z9e3j2 \
    --discovery-token-ca-cert-hash sha256:40a70e4054a6670bfc22bc7b975e6ec1a0e0a9b749c8d14a5df251f18f30509b
```

Aguarde alguns segundos, execute o comando abaixo e você verá alguns servidores sem role definida e o status `Ready`, agora temos um cluster com dois masters e quatro workers.


* Configuração de acesso limitado do usuário `desenvolvedor` 

Para esse cluster foi configurado um acesso limitado para o usuário `desenvolvedor`, no qual o mesmo terá permissão apenas de `get`, `list`  e `watch` em alguns recursos do cluster, vamos configurar:

Service Account:

```bash
cat > serviceaccount-dev.yaml <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: readonlyuser
  namespace: default
EOF
```

```bash
kubectl create -f serviceaccount-dev.yaml 
```

Cluster Role:

```bash
cat > clusterrole-dev.yaml.yaml <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: readonlyuser
rules:
- apiGroups:
  - ""
  resources:
  - pods
  - services
  - nodes
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - apps
  resources:
  - daemonsets
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - extensions
  resources:
  - ingresses
  verbs:
  - get
  - list
  - watch
EOF
```

```bash
kubectl create -f clusterrole-dev.yaml.yaml 
```

Service Account:

```bash
cat > clusterrolebinding-dev.yaml <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: readonlyuser
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: readonlyuser
subjects:
- kind: ServiceAccount
  name: readonlyuser
  namespace: default
EOF
```

```bash
kubectl create -f clusterrolebinding-dev.yaml 
```

Os arqvuivos de configuração RBAC estão em `files/voting-app/k8s-rbac-dev/config-dev`. Configurações RBAC realizadas, iremos agora configurar o acesso do usuário `desenvolvedor` no cluster:

Recuperando Token do Service Account

```bash
TOKEN=$(kubectl describe secrets "$(kubectl describe serviceaccount readonlyuser | grep -i Tokens | awk '{print $2}')" | grep token: | awk '{print $2}')
```

Configurando Token para o usuário `desenvolvedor`

```bash
kubectl config set-credentials desenvolvedor --token=$TOKEN
```

Configurando contexto de acesso do usuário `desenvolvedor`

```bash
kubectl config set-context devreader --cluster=kubernetes --user=desenvolvedor
```

Usuário e acessos configurado, foi então configurado o arquivo `~/.kube/config` do usuário `desenvolvedor` dentro do perfil de usuário, o arquivo de configuração está em `files/voting-app/k8s-rbac-dev/config-dev`

Check de permissões do usuário `desenvolvedor`

![dev-access](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/check-access-dev.png)

## Voting App

Como informado no início desse repositório, foi solicitado que a aplicação [Example Voting App](https://github.com/dockersamples/example-voting-app) funcionasse em um ambiente de cluster kubernetes, dessa forma já temos um cluster kubernetes pronto e iremos configurar aplicação, para isso primeiro iremos criar o namespace chamado `vote`:

```bash
cat > vote-namespace.yaml <<EOF
apiVersion: v1
kind: Namespace
metadata:
  name: vote
EOF
```
```bash
kubectl create -f vote-namespace.yaml
```

Antes de iniciar a aplicação deve-se informar que os `Deployments` oringinais [vote](https://vote.hebersonaguiar.me), [result](https://result.hebersonaguiar.me) e [worker]((https://worker.hebersonaguiar.me)) foram alterados para `DaemonSet` para que cada worker possua um pod de cada umas das aplicações informadas, isso faz com que o Load Balancer criado com [Nginx](https://github.com/hebersonaguiar/getupclouddocs#nginx) funcione corretamente.

Para iniciar a aplicação baixe os arquivos desse repositório em um dos masters:

```bash
git clone https://github.com/hebersonaguiar/getupclouddocs.git
```

Inicie a aplicação com o comando abaixo:

```bash
kubectl create -f files/voting-app/k8s-voting/
```

Realizado o comando acima, serão inicializados, `replicaset`, `deployment`, `daemonset`, `service` e os `pods` das aplicações `vote`, `result`, `worker` da base de dados e do redis, como mostra a imagem abaixo:

![votingapp](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/voting-app.png)

Como mostra a imagem acima, temos dois serviços do tipo `NodePort` que são das aplicações [vote](https://vote.hebersonaguiar.me) e [result](https://result.hebersonaguiar.me), ou seja, o acessos dessas aplicações podem ser acessadas diretamente dos nós workers nas portas `31000` e `31001` respectivamente. Essas portas foram configuradas no Load Balancer do [Nginx](https://github.com/hebersonaguiar/getupclouddocs#nginx) e onde podemos acessar e brincar com as aplicações:

* Vote

![vote](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/vote.png)

* Result

![result](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/result.png)

## Helm Chart
O Helm Chart é um gerenciador de aplicações Kubernetes onde cria, versiona, compartilha e pública os artefatos. Com ele é possível desenvolver templates dos arquivos YAML e durante a instalação de cada aplicação personalizar os parâmentros com facilidade. 

Nesse projeto o helm chart foi utilizado para deploy das aplicações [prometheus](https://prometheus.hebersonaguiar.me), [grafana](https://grafana.hebersonaguiar.me), [alertmanager](https://alertmanager.hebersonaguiar.me) e a stack do elastic acessando pelo [kibana](https://kibana.hebersonaguiar.me).

Antes de iniciar as aplicações é necessário a configuração do `helm` e `tiller`:

Esse passo deve ser executado em todos os masters:
```bash
cd /opt
wget https://get.helm.sh/helm-v2.16.1-linux-amd64.tar.gz
tar -zxvf helm-v2.16.1-linux-amd64.tar.gz
mv linux-amd64/helm /usr/local/bin/helm
```

Configuração do Tiller, executar em apenas um nó:
```bash
kubectl -n kube-system create serviceaccount tiller

kubectl create clusterrolebinding tiller \
  --clusterrole=cluster-admin \
  --serviceaccount=kube-system:tiller

helm init --service-account tiller

kubectl -n kube-system  rollout status deploy/tiller-deploy

helm version --short
Client: v2.16.1+gbbdfe5e
Server: v2.16.1+gbbdfe5e
```

## GitHub
GitHub é uma plataforma de hospedagem de código-fonte com controle de versão usando o Git. Ele permite que programadores, utilitários ou qualquer usuário cadastrado na plataforma contribuam em projetos privados e/ou Open Source de qualquer lugar do mundo.

Nesse projeto foi utilizado para os repositórios das aplicações [Vote](https://github.com/hebersonaguiar/vote), [Worker](https://github.com/hebersonaguiar/worker), [Result](https://github.com/hebersonaguiar/result) e para essa documentação [GetUp Cloud Docs](https://github.com/hebersonaguiar/getupclouddocs)

## Prometheus
O Prometheus é um kit de ferramentas de monitoramento e alerta de sistemas de código aberto criado originalmente no SoundCloud. Desde a sua criação em 2012, muitas empresas e organizações adotaram o Prometheus, e o projeto possui uma comunidade de desenvolvedores e usuários muito ativa. Agora é um projeto de código aberto independente e mantido independentemente de qualquer empresa.

Iremos utilizar nesse projeto o prometheus para coletar dados do cluster kubernetes bem como as aplicações, para sua instalação iremos utilizar o helm chart, por ser uma aplicação de terceiro e também por está mantida sua versão estável no repositório de charts no github, para sua instalação iremos utilizar o comando abaixo:

Antes das instalação iremos criar o ` namespace` que irá alocar nossas aplicações de minitoramento e logs:

```bash
kubectl create ns observability
``` 

Serão implantadas o [prometheus](https://prometheus.hebersonaguiar.me) e [alertmanager](https://alertmanager.hebersonaguiar.me), no qual são inciadas juntas, nelas serão configuradas a porta de serviço como `NodePort` e desabilitar o persistente volume, segue abaixo a instalação:

```bash
helm install --name prometheus --namespace observability --set \
	alertmanager.persistentVolume.enabled=false,server.persistentVolume.enabled=false, \
	alertmanager.service.type=NodePort,server.service.type=NodePort \
	stable/prometheus
```

[Prometheus](http://prometheus.hebersonaguiar.me/graph) em execução:

![prometheus](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/prometheus.png)


## Grafana
Grafana é uma suíte de análise e visualização métrica de código aberto. É mais comumente usado para visualizar dados de séries temporais para análise de infraestrutura e aplicativos.

Nesse projeto iremos instalar o grafana e configurá-lo para conectar-se ao prometheus e configurar dashboards de métricas do cluster e as aplicações, para isso iremos utilizar o helm chart,  para sua instalação iremos utilizar o comando abaixo:

* Criação de um namespace para o observability (caso não exista)

```bash
kubectl create ns observability
```

Será implantado o [grafana](https://grafana.hebersonaguiar.me), no qual irá se conectar ao prometheus e vai nos trazer as métricas do ambiente como uso de disco, memória, cpu, etc, tudo em um dashboard que iremos configurar posteriormente, para iniciar iremos configuar apenas o serviço como `NodePort`:

```bash
helm install --name grafana --namespace observability --set service.type=NodePort stable/grafana
```
Para resgatar a senha do usuário `admin` do grafana utilize o comando abaixo: 

```bash
kubectl get secret --namespace observability grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

[Grafana](https://grafana.hebersonaguiar.me) em execução:

![grafana](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/grafana.png)


Os acessos ao grafana são:

`Usuário: admin`

`Senha: yXRXQ3RFb143zGf7bq4N2v6phlfI2rUKjsKo2X3V`


* Criação de datasource de conexão com prometheus:

A criação da conexão do grafana com o prometheus é em Datasource > Prometheus, ao clicar vai abrir um formulário para preenchimento, segue abaixo as inforações para preenchimento:

`Nome: Prometheus`

`URL de conexão com prometheus: https://prometheus.hebersonaguiar.me`

Pronto, é só clicar em testar e salvar.
 

* Importação de dashboards

Dentro da pasta `files/grafana/` possui um arquivo do tipo `.json` que são os dashboards que iremos utilizar, no qual mostra todas as infomrações do cluster, como, uso de memória, CPU, disco, etc, são métricas do cluster e dos contêiners. 

A importação de um dashboard é bem simples, no canto superior esquerdo, possui um ícone com o nome Home, ao clicar será aberto um modal com algumas informações, entre elas a `Import Dashboard`, ao clicar uma nova página é aberta e algumas informações são solicitadas, que são como que o dashboard vai ser importado nele possui três formas, upload de um arquivo `.json`, o ID de um dashboard público ou colar o conteúdo de um arquivo `.json`, nesse caso, como temos um dashboard customizado iremos colar o contéudo do nosso arquivo `.json`, feito isso basta apenas clicar em load.

Agora para visualizar, basta ir em Home, clicar no dashboard "Kubernetes Cluster - Monitoramento" e visualizar os dados, como mostra a imagem abaixo:

![grafana](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/grafana-dash.png)


## Elastic Stack
O Elastic Stack é um conjunto de aplicações que nos ajudam a ter uma melhor visialização dos logs de ambientes, nesse projeto iremos configurar essas aplicações para que possamos ter os logs de nosso cluster.

Iremos instalar o Elastic Stack utilizando o helm chart, segue abaixo:

* Criação de um namespace para o observability (caso não exista)

```bash
kubectl create ns observability
```

* Elasticssearch

O Elasticsearch é um mecanismo de pesquisa e análise de código aberto distribuído para todos os tipos de dados, incluindo texto, numérico, geoespacial, estruturado e não estruturado.
Primiero passo para instalação é a adição do repositório de charts do elastic:

```bash
helm repo add elastic https://helm.elastic.co
```

Posteriormente vamos iniciar o elasticsearch desabilitando o persistente volume:

```bash
helm install --name elasticsearch --namespace observability --set persistence.enabled=false elastic/elasticsearch
```

Após iniciar o elasticsearch podemos então iniciar o kibana configurando o serviço como `NodePort`:

```bash
helm install --name kibana --namespace observability --set service.type=NodePort elastic/kibana
```

Para finalizar precisamos configurar o metricbeat para que possamos resgatar os logs e seus respectivos índices:

```bash
helm install --name metricbeat --namespace observability elastic/metricbeat
```

Com a stack implantada temos o seguinte resultado no cluster kubernetes:

![elasticstack](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/elasticstack.png)

Pronto, agora basta acessar o [Kibana](http://kibana.hebersonaguiar.me), configurar os index do metricbeat e pronto.

![elk](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/kibana.png)

## Nginx

Para esse projeto foi utilizado o NGiNX como proxy e load balancer das aplicações [vote](https://vote.hebersonaguiar.me), [result](https://result.hebersonaguiar.me) [prometheus](https://prometheus.hebersonaguiar.me), [alertmanager](https://alertmanager.hebersonaguiar.me), [kibana](http://kibana.hebersonaguiar.me) e o [grafana](http://grafana.hebersonaguiar.me)

Para configurar foi realizado os seguintes passos:

Acesso ao servidor `haproxy.hebersonaguiar.me`

```bash
ssh -i <path-cahve_ssh/id_rsa> -l hebersonaguiar_ti haproxy.hebersonaguiar.me
```

Instalação do NGiNX:

```bash
apt-get -y install nginx
```

Configuraçõe dos Vhosts:

* Vote
```bash
cat > /etc/nginx/conf.d/vote-lb.conf <<EOF
upstream backend_vote {
   server 10.128.0.44:31000; 
   server 10.168.0.2:31000;
   server 10.138.0.8:31000;
   server 10.142.0.9:31000;
}

server {
    listen 80;
    server_name vote.hebersonaguiar.me; 

    location / {
    return 301 https://vote.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name vote.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_vote;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}
EOF
```

* Result
```bash
cat > /etc/nginx/conf.d/result-lb.conf <<EOF
upstream backend_result {
   server 10.128.0.44:31001; 
   server 10.168.0.2:31001;
   server 10.138.0.8:31001;
   server 10.142.0.9:31001;
}

server {
    listen 80;
    server_name result.hebersonaguiar.me; 

    location / {
    return 301 https://result.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name result.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_result;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}
EOF
```

* Prometheus
```bash
cat > /etc/nginx/conf.d/prometheus.conf <<EOF
upstream backend_prometheus {
   server 10.128.0.44:32295; 
}

server {
    listen 80;
    server_name prometheus.hebersonaguiar.me; 

    location / {
    return 301 https://prometheus.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name prometheus.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_prometheus;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}
EOF
```

* Alert Manager
```bash
cat > /etc/nginx/conf.d/alertmanager.conf <<EOF
upstream backend_alertmanager {
   server 10.138.0.8:31379; 
}

server {
    listen 80;
    server_name alertmanager.hebersonaguiar.me; 

    location / {
    return 301 https://alertmanager.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name alertmanager.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_alertmanager;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}
EOF
```

* Kibana
```bash
cat > /etc/nginx/conf.d/kibana.conf <<EOF
upstream backend_kibana {
   server 10.168.0.2:30103; 
}

server {
    listen 80;
    server_name kibana.hebersonaguiar.me; 

    location / {
    return 301 https://kibana.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name kibana.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_kibana;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}
EOF
```

* Grafana
```bash
cat > /etc/nginx/conf.d/grafana.conf <<EOF
upstream backend_grafana {
   server 10.142.0.9:30693; 
}

server {
    listen 80;
    server_name grafana.hebersonaguiar.me; 

    location / {
    return 301 https://grafana.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name grafana.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_grafana;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}
EOF
```

Os arquivos acima estão em `files/nginx`. Os ips configurados nos vhosts são dos workes do cluster kubernetes.

Verificação de sintaxe

```bash
nginx -t
```

Restart do serviço

```bash
systemctl restart nginx
```