#### LAB03 - Instalando OCP {#_lab03_instalando_ocp}

Demo da instalacao avançada e instalacao do OC Cluster UP

##### 1 - Assistir a demo {#_1_assistir_a_demo}

##### 2 - Instalar o OC Cluster UP {#_2_instalar_o_oc_cluster_up}

```
yum install centos-release-openshift-origin
yum install origin-clients
```

##### 3 - Fixa problemas com docker {#_3_fixa_problemas_com_docker}

```
vi /etc/sysconfig/docker
systemctl start docker
```

```
Editar a linha de OPTIONS e adicionar --insecure-registry 172.30.0.0/1
Ex:OPTIONS='--selinux-enabled --log-driver=journald --signature-verification=false --insecure-registry 172.30.0.0/16'
```

##### 4 - Sobe OC cluster up {#_4_sobe_oc_cluster_up}

```
oc cluster up --public-hostname=<ip publico>
```

##### 5 - Criar novo projeto {#_5_criar_novo_projeto}

```
Acessa a interface com <logincriado> <senha redhat > 
https://openshift.example.com:8443
Clicar no botao "Create Project" e crie um projeto com o <logincriado>
```

```
Caso a instalacao tenha sido feita na propria maquina, Acessar https://ipdoservidor:8443 e entrar com login developer e senha  qualquer
```

##### 6 - Cria nova app {#_6_cria_nova_app}

```
Acessa o objeto criado
Clicar no botao "Browse Catalog"
Selecione php
Selecione seu projeto, versao do php desejada e em git repository coloque:
https://bitbucket.org/redhatbsb/phpapp.git
Caso tenha erro em deployment, executar: oc adm policy add-scc-to-user anyuid -z default -n <nomedoprojeto>
```

 

