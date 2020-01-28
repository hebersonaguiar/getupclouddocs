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

[Kubernetes Engine](https://github.com/hebersonaguiar/getupclouddocs#kubernetes)

[Helm Chart](https://github.com/hebersonaguiar/getupclouddocs#helm-chart)

[GitHub](https://github.com/hebersonaguiar/getupclouddocs#github)

[Prometheus](https://github.com/hebersonaguiar/getupclouddocs#prometheus)

[Grafana](https://github.com/hebersonaguiar/getupclouddocs#grafana)

[ELK](https://github.com/hebersonaguiar/getupclouddocs#elk)


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
`Senha: yXRXQ3RFb143zGf7bq4N2v6phlfI2rUKjsKo2X3V`

Vote, Result, Prometheus e Kibana possuem acesso livre. 

Para autenticar nos servidores é necessário utilizar chaves para isso utilize essa chave:

[Chave SSH](https://github.com/hebersonaguiar/getupclouddocs/blob/master/files/id_rsa)

Para autenticar utilizando terminal linux utilize comando abaixo:

Usuário Admin `hebersonaguiar_ti`

```bash
ssh -i <path-cahve_ssh/id_rsa> hebersonaguiar_ti <nome-do-servidor>
```

Usuário Desenvolvedor `desenvolvedor`

```bash
ssh -i <path-cahve_ssh/id_rsa> desenvolvedor <nome-do-servidor>
```

## Docker
O Docker é uma plataforma para desenvolvedores e administradores de sistemas para desenvolver, enviar e executar aplicativos. O Docker permite montar aplicativos rapidamente a partir de componentes e elimina o atrito que pode ocorrer no envio do código. O Docker permite que seu código seja testado e implantado na produção o mais rápido possível. 

## Google Cloud Plataform
Google Cloud Platform também conhecida como GCP é uma suíte de cloud oferecida pelo Google, funcionando na mesma infraestrutura que a empresa usa para seus produtos dirigidos aos usuários, dentre eles o Buscador Google e o Youtube.
Para esse projeto foi utilizada uma conta pessoal. Os produtos que iremos utilizar são 

* [Google Cloud Plataform](https://github.com/hebersonaguiar/getupclouddocs#google-cloud-plataform), onde foi utilizado o domínio `hebersonaguiar.me` apontando os momes das aplicações para os serviços de entrada de requisição, e também os DNS dos servidores.

* [Compute Engine](https://github.com/hebersonaguiar/getupclouddocs#google-cloud-plataform), onde foi criado as intâncias de vm do cluster composto por dois master, 4 workers e 1 haproxy e nginx. A criação das instâncias foram feitas manualmente, devido a divisão do cluster em relação ao tempo, ou seja, para criação utilizando como por exemplo `terraform` iria levar mais tempo para criar os arquivos e configurações do que criar manualmente pois as instâncias foram dividias em zonas e tipo.

* [Kubernetes](https://github.com/hebersonaguiar/getupclouddocs#kubernetes), utilizado para criar o cluster do Kubernetes. Nesse projeto foi criado um cluster multi-master para que possa garantir a disponibilidade do cluster caso um dos master pare de funcionar. Foi também utilizado o `kubeadm` para criação do cluster, as informações de criação de configuração do cluster estão na seção [Kubernetes](https://github.com/hebersonaguiar/getupclouddocs#kubernetes)


Criação das Instâncias:

As instâncias foram criadas com as seguintes configurações:

Hosts `tom`, `jerry`, `haproxy`, `charliebrown` foram criadas na zona `us-central-a` com 2 vCPUs, 7,5 GB de memória (n1-standard-2);

Host `pernalonga`, foi criada na zona `us-west1-a` com 2 vCPUs, 7,5 GB de memória (n1-standard-2);

Host `popeye`, foi criada na zona `us-east1-a` com 2 vCPUs, 7,5 GB de memória (n1-standard-2);

Host `patolino`, foi criada na zona `us-west2-c` com 1 vCPUs, 3,75 GB de memória (n1-standard-1);

## Kubernetes

Kubernetes ou como é conhecido também K8s é um produto Open Source utilizado para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêiner no qual agrupa contêineres que compõem uma aplicação em unidades lógicas para facilitar o gerenciamento e a descoberta de serviço

Nesse projeto foi utilizado o kubernetes na versão `v1.13.7-gke.24`, o cluster foi criado na GCP, o Kubernetes Engine não é voltado apenas para aplicativos sem estado. Com ele, é possível incluir armazenamento permanente e até mesmo executar um banco de dados no seu cluster.

A instação do cluster kubernetes foi realizada pelo Jenkins X, no qual possui uma forte integração com a GCP, criando um ambinete de CI/CD de forma rápida e fácil.

O cluster criado contém três nós, como mostra a imagem abaixo:

![k8s nodes](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/k8s-nodes.png)





## Helm Chart
O Helm Chart é um gerenciador de aplicações Kubernetes onde cria, versiona, compartilha e pública os artefatos. Com ele é possível desenvolver templates dos arquivos YAML e durante a instalação de cada aplicação personalizar os parâmentros com facilidade. 

Nesse projeto o helm chart foi utilizado nos repositórios das aplicações [Frontend](https://github.com/hebersonaguiar/ditochatfrontend/tree/master/charts/ditochatfrontend) e [Backend](https://github.com/hebersonaguiar/ditochatbackend/tree/master/charts/ditochatbackend), no qual foi emcapsulado todos os arquivos necessários para a implantação das aplicações, como deployment, service, persistente volume, etc, um template padrão de uma aplicação é a seguinte:

![helm chart template](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/helm-chart-temp.png)

Dentro do diretório de charts, existe um arquivo chamado `values.yaml`, muito importante dentro de um helm chart, ele é o resposável por informar para os arquivios YAML quais os valores que serão alterados que podem ser por exemplo: quantidade de replicas, portas de acesso, tipo de deploy, tamanho do volume a ser utilizado, etc.


## GitHub
GitHub é uma plataforma de hospedagem de código-fonte com controle de versão usando o Git. Ele permite que programadores, utilitários ou qualquer usuário cadastrado na plataforma contribuam em projetos privados e/ou Open Source de qualquer lugar do mundo.

## Prometheus
O Prometheus é um kit de ferramentas de monitoramento e alerta de sistemas de código aberto criado originalmente no SoundCloud. Desde a sua criação em 2012, muitas empresas e organizações adotaram o Prometheus, e o projeto possui uma comunidade de desenvolvedores e usuários muito ativa. Agora é um projeto de código aberto independente e mantido independentemente de qualquer empresa.

Iremos utilizar nesse projeto o prometheus para coletar dados do cluster kubernetes bem como as aplicações, para sua instalação iremos utilizar o helm chart, por ser uma aplicação de terceiro e também por está mantida sua versão estável no repositório de charts no github, para sua instalação iremos utilizar o comando abaixo:

* Criação de um namespace para o monitoramento e log (caso não exista)

```bash
kubectl create namespace monitoring-log
```

* Instalação do prometheus

```bash
helm install --name prometheus \
	--set server.persistentVolume.enabled=false,alertmanager.persistentVolume.enabled=false \
	--namespace monitoring-log \
	stable/prometheus
```

Após todos os serviços estarem funcionando, será necessário criar um ingress para o acesso externo, para isso iremos utilizar o comando abaixo:

```bash
kubectl create -f ingress-prometheus.yaml
```
O arquivo de configuação do ingress encontra-se em `conf/k8s/`

[Prometheus](http://prometheus.ditochallenge.com/graph) em execução:

![prometheus](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/prometheus.png)


## Grafana
Grafana é uma suíte de análise e visualização métrica de código aberto. É mais comumente usado para visualizar dados de séries temporais para análise de infraestrutura e aplicativos.

Nesse projeto iremos instalar o grafana e configurá-lo para conectar-se ao prometheus e configurar dashboards de métricas do cluster e as aplicações, para isso iremos utilizar o helm chart,  para sua instalação iremos utilizar o comando abaixo:

* Criação de um namespace para o monitoramento e log (caso não exista)

```bash
kubectl create namespace monitoring-log
```

* Instalação do grafana

```bash
helm install --name grafana \
	--set persistence.enabled=false \
	--namespace monitoring-log \
	 stable/grafana
```

Após todos os serviços estarem funcionando, será necessário criar um ingress para o acesso externo, para isso iremos utilizar o comando abaixo:

```bash
kubectl create -f ingress-grafana.yaml
```
O arquivo de configuação do ingress encontra-se em `conf/k8s/`

[Grafana](http://grafana.ditochallenge.com) em execução:

![grafana](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/grafana.png)

Na instalação do grafana é criado uma senha de acesso, para consulta-la execute o comando abaixo:

```bash
kubectl get secret --namespace monitoring-log grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Os acessos ao grafana são:

`Usuário: admin`

`Senha: gc5iMZLszrPVwLaJyEGxEf7nNvxL7uq0YGkLQU4p`

Criação de datasource de conexão com prometheus:

A criação da conexão do grafana com o prometheus é em Datasource > Prometheus, ao clicar vai abrir um formulário para preenchimento, segue abaixo as inforações para preenchimento:

`Nome: Prometheus`

`URL de conexão com prometheus: http://prometheus.ditochallenge.com`

Pronto, é só clicar em testar e salvar.
 

* Importação de dashboards

Dentro da pasta `conf/grafana/` possui alguns arquivos do tipo `.json` que são os dashboards que iremos utilizar, no qual mostra todas as infomrações do cluster, como, uso de memória, CPU, disco, etc, são métricas do cluster e dos contêiners. 

A importação de um dashboard é bem simples, no canto superior esquerdo, possui um ícone com o nome Home, ao clicar será aberto um modal com algumas informações, entre elas a `Import Dashboard`, ao clicar uma nova página é aberta e algumas informações são solicitadas, que são como que o dashboard vai ser importado nele possui três formas, upload de um arquivo `.json`, o ID de um dashboard público ou colar o conteúdo de um arquivo `.json`, nesse caso, como temos um dashboard customizado iremos colar o contéudo do nosso arquivo `.json`, feito isso basta apenas clicar em load.

Agora para visualizar, basta ir em Home, clicar no dashboard "Kubernetes Cluster - Monitoramento" e visualizar os dados, como mostra a imagem abaixo:

![grafana](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/grafana-monitoring.png)


## ELK
ELK signifca ELasticsearch, Logstash e Kíbana, um conjunto de aplicações que nos ajudam a ter uma melhor visialização dos logs de ambientes, nesse projeto iremos configurar essas aplicações para que possamos ter os logs de nosso cluster.

Na instalação do ELK não iremos utilizar o Helm Chart, vamos fazer urilizando o próprio Kubernetes, todas as configurações que iremos utilizar aqui estão em `conf/k8s/elk`.

* Elasticssearch

O Elasticsearch é um mecanismo de pesquisa e análise de código aberto distribuído para todos os tipos de dados, incluindo texto, numérico, geoespacial, estruturado e não estruturado.
Primiero passo para instalação é a criação do deployment, no qual irá ser criado um pod do elasticsearch, para isso execute o comando abaixo:

```bash
kubectl create -f deployment-elastic.yaml
```

Após executar o comando, aguarde enquanto o pod é iniciado, após sua inicialização precisamos criar um serviço para o pod do elasticsearch, isso fará que o Logstash possa se comunicar com ele, para isso execute o comando abaixo:

```bash
kubectl create -f service-elastic.yaml
```

* Kibana

O Kibana permite visualizar os dados do Elasticsearch  para você poder fazer qualquer coisa, desde rastrear a carga de consultas até entender a maneira como as solicitações fluem pelos aplicativos.
O Kibana será nosso frontend dos logs, ou seja, iremos poder ver os logs, gráficos de logs de nosso cluster a partir dele, para sua instalação iremos executar os seguintes comandos:

```bash
kubectl create -f deployment-kibana.yaml
```

Após executar o comando, aguarde enquanto o pod é iniciado, após sua inicialização precisamos criar um serviço para o pod do kibana, execute o comando abaixo:

```bash
kubectl create -f service-kibana.yaml
```

Após a criação do serviço do kibana, iremos agora criar um ingress, o ingress irá nos permitir acessar o painel do kibana, execute o comando abaixo:

```bash
kubectl create -f ingress-kibana.yaml
```

* Logstash

O Logstash é um pipeline de processamento de dados open source do lado do servidor que faz a ingestão de dados a partir de inúmeras fontes simultaneamente, transforma-os e envia-os para o seu "esconderijo" favorito.
Para esse projeto iremos utilizar o Fluentd como Logstash, ele vai se conectar ao elasticsearch e coletar os dados, para isso iremos executar os comandos abaixo:

O comando abaixo cria umm RBAC (role-based access control), um controle de acesso para o Fluentd possa acessar corretamente todos os componentes do cluster.

```bash
kubectl create -f fluentd-rbac.yaml
```

Criado as permissões, agora podemos criar um DaemonSet, diferente do deployment, o DaemonSet fará com que todos os nós obrigatoriamente contenha um pod do Fluentd, isso é importante pois todos os nós precisam de um mecanismo que possa coletar os dados, segue abaixo o comando:

```bash
kubectl create -f fluentd-daemonset.yaml
```
Pronto, agora basta acessar o [Kibana](http://kibana.ditochallenge.com), configurar os index do logstash e pronto.

![elk](https://github.com/hebersonaguiar/getupclouddocs/blob/master/images/kibana-access.png)