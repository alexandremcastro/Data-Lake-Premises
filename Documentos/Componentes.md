### Sumário
+ [Instalação e configuração do Java JDK 8](#Java)
+ [Instalação e configuração do Apache Hadoop](#Hadoop)
    + [Configuração do Hadoop](#ConfiguracaoHadoop)
    + [Inicialização do Hadoop](#InicializacaoHadoop)
+ [Instalação e configuração do Apache Kafka](#Kafka)
+ [Instalação e configuração do Apache Nifi](#Nifi)
+ [Instalação e configuração do Apache Spark](#Spark)
    + [Configurando o Spark em Multinode Cluster](#Cluster)
+ [Instalação e configuração do Apache Hive](#Hive)
    + [Configuração do Metastore com Banco de Dados Oracle](#Oracle)
    + [Configuração do Metastore com Banco de Dados MySQL](#MySQL)
    + [Testando o Hive](#HiveTest)

<a name = "Java"></a>

### Instalação e configuração do Java JDK 8

Maior parte das aplicações da Apache roda sobre o Java, para isso devemos instalar o Java JDK, estarei utilizando a versão 8 porque é compatível com maior parte dos componentes, por mais que a versão do Hadoop atual suporte o Java 11.
 
> **💡**: Certifique-se de que todas as máquinas estão ligadas.

<b>Download do Java 8</b>

Para isso é necessário baixar o Java JDK 8 através do seguinte link:

[https://www.oracle.com/br/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/br/java/technologies/javase/javase8-archive-downloads.html)

Estarei selecionando a versão de Linux x64 compactada.

Baixe a opção na sua máquina local:

- jdk-8u202-linux-x64.tar.gz

> **💡**: Estarei selecionando essa opção, mas não é obrigatório, baixe a compatível com a sua máquina.

![Untitled](/Imagens/Untitled%2052.png)

Será solicitado o login, caso não tenha, faça um cadastro no site da Oracle.

[https://profile.oracle.com/myprofile/account/create-account.jspx](https://profile.oracle.com/myprofile/account/create-account.jspx)

![Untitled](/Imagens/Untitled%2053.png)

Após o login o Download será iniciado automaticamente.

<br>

<b>Copiando o Java para as máquinas</b>

Depois de baixar, faremos a cópia desse arquivo.

Abra o terminal da sua máquina local, onde foi baixado o Java e execute o comando:

```bash
scp /caminho/jdk-8u202-linux-x64.tar.gz root@ipdonamenode:/opt
```

![Untitled](/Imagens/Untitled%2054.png)

Faça a cópia do Java para os Datanodes também

```bash
scp /caminho/jdk-8u202-linux-x64.tar.gz root@ipdodatanode:/opt
```

![Untitled](/Imagens/Untitled%2055.png)

Após a execução, verifique se a cópia realmente foi feita. Acesse o diretório `/opt/` das máquinas e execute o comando `ls -la` para verificar se o arquivo foi realmente copiado

![Untitled](/Imagens/Untitled%2056.png)

Após verificar estamos preparados para instalar o Java.

> **💡**: É necessário realizar todos passos seguintes em todas as máquinas.

### Instalação do Java 8 

Para a instalação, o primeiro passo é extrair o conteúdo do arquivo jdk-8u202-linux-x64.tar.gz,

para isso utilizaremos o comando, responsável por extrair os arquivos:

```bash
sudo tar xzf jdk-8u202-linux-x64.tar.gz
```

Verificando se foi extraído corretamente:

```bash
ls -la
```

![Untitled](/Imagens/Untitled%2057.png)

Agora irei apagar o arquivo compactado, renomear o diretório criado para JDK e alterar o dono e grupo, seguindo uma boa prática de padronização.

```bash
sudo rm -rf jdk-8u202-linux-x64.tar.gz
sudo mv jdk1.8.0_202/ /opt/jdk
ls -la
sudo chown -R root:root jdk/
```

![Untitled](/Imagens/Untitled%2058.png)

Para dar continuidade a instalação, precisa configurar as variáveis de ambiente, para isso edite o arquivo `.bash_profile`, localizado no `~/`

Insira as seguintes configurações no arquivo, indicando o caminho do Java:

```bash
export JAVA_HOME=/opt/jdk
export JRE_HOME=/opt/jdk/jre
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```

![Untitled](/Imagens/Untitled%2059.png)

Para atualizar o arquivo de variáveis de ambiente rode o comando:

```bash
source .bash_profile
```

Para verificar se tudo está configurado corretamente, rode o comando 

```bash
java -version
```

![Caso apareça a versão, significa que tudo está corretamente configurado.](/Imagens/Untitled%2060.png)

Caso apareça a versão, significa que tudo está corretamente configurado.

Agora é possível a instalação do Hadoop.

<br>

<a name = "Componentes"></a>
## Instalando e configurando os componentes

<br>
<a name = "Hadoop"></a>

### Instalação e configuração do Apache Hadoop

> **Nota**: Para a instalação do Hadoop, certifique que todas as máquinas estejam iniciadas e com o Java JDK instalado.

Primeiro passo é fazer o Download do arquivo binário do Hadoop 3.3.4:

[https://hadoop.apache.org/releases.html](https://hadoop.apache.org/releases.html)

![Untitled](/Imagens/Untitled%2061.png)

Copie o link de Download

[https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz](https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz)

![Untitled](/Imagens/Untitled%2062.png)

Abra o terminal do Namenode, vá ao diretório `/opt/` e insira o comando `wget` com o link do Hadoop:

> **💡**: Só será possível a execução do comando wget, caso você tenha ele instalado.

```bash
sudo wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz
```

![Untitled](/Imagens/Untitled%2063.png)

Verificando, extraindo, removendo o arquivo compactado e alterando o nome do diretório.

```bash
ls -la
sudo tar xzf hadoop-3.3.4.tar.gz
sudo rm -rf hadoop-3.3.4.tar.gz
sudo mv hadoop-3.3.4/ /opt/hadoop
```

![Untitled](/Imagens/Untitled%2064.png)

Adotando novamente outro padrão, estarei colocando no usuário datalake todas as instalações dos componentes Apache.

```bash
sudo chown -R datalake:datalake hadoop/
```

![Untitled](/Imagens/Untitled%2065.png)

Agora irei configurar as variáveis de ambiente do meu usuário datalake inserindo as seguintes configurações:

```bash
export HADOOP_HOME=/opt/hadoop
export HADOOP_CONF_DIR="${HADOOP_HOME}/etc/hadoop"
export PATH=$PATH:$HADOOP_HOME/bin
```

![Untitled](/Imagens/Untitled%2066.png)

Agora podemos testar a versão do Hadoop, caso as variáveis de ambiente tenham sido corretamente configuradas.

```bash
hadoop version
```

![Untitled](/Imagens/Untitled%2067.png)

<br>

<a name = "ConfiguracaoHadoop"></a>
<b> Configuração do Hadoop</b>

Após a instalação e configuração das variáveis de ambiente, precisamos configurar o Hadoop, para isso é necessário entrar no diretório: `/opt/hadoop/etc/hadoop`

Começarei configurando o arquivo `hadoop-env.sh`, insira ou habilite as seguintes linhas no arquivo:

```bash
export JAVA_HOME=/opt/jdk
export HADOOP_HOME=/opt/hadoop
export HADOOP_CONF_DIR="${HADOOP_HOME}/etc/hadoop"
export PATH="${PATH}:${HADOOP_HOME}/bin"
export HADOOP_SSH_OPTS="-i ~/.ssh/id_rsa"
```

![Untitled](/Imagens/Untitled%2068.png)

Configurando o arquivo `core-site.xml`

Apague as tags `configuration`.

> **💡**: Muito cuidado apagando este tipo de arquivo para não apagar as indentações.

![Untitled](/Imagens/Untitled%2069.png)

Insira as seguintes configurações:

```html
<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://namenode:19000</value>
    </property>
</configuration>
```

![Untitled](/Imagens/Untitled%2070.png)

Configurando o `hdfs-site.xml`

Insira as seguintes configurações:

```html
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/opt/hadoop/dfs/namespace_logs</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/opt/hadoop/dfs/data</value>
    </property>
</configuration>
```

![Untitled](/Imagens/Untitled%2071.png)

Configurando o `mapred-site.xml`

Insira as seguintes configurações:

```html
<configuration>
    <property>
        <name>mapreduce.job.user.name</name>
        <value>hadoop</value>
    </property>
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>namenode:8032</value>
    </property>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/hadoop</value>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/hadoop</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=/opt/hadoop</value>
    </property>
</configuration>
```

![Untitled](/Imagens/Untitled%2072.png)

Configurando o `yarn-site.xml`

Insira as seguintes configurações:

```html
<configuration>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>namenode</value>
    </property>
    <property>
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>1536</value>
    </property>
    <property>
        <name>yarn.scheduler.maximum-allocation-mb</name>
        <value>1536</value>
    </property>
    <property>
        <name>yarn.scheduler.minimum-allocation-mb</name>
        <value>128</value>
    </property>
    <property>
        <name>yarn.nodemanager.vmem-check-enabled</name>
        <value>false</value>
    </property>
    <property>
        <name>yarn.server.resourcemanager.application.expiry.interval</name>
        <value>60000</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>
    <property>
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>-1</value>
    </property>
    <property>
        <name>yarn.application.classpath</name>
        <value>$HADOOP_CONF_DIR,${HADOOP_COMMON_HOME}/share/hadoop/common/*,${HADOOP_COMMON_HOME}/share/hadoop/common/lib/*,${HADOOP_HDFS_HOME}/share/hadoop/hdfs/*,${HADOOP_HDFS_HOME}/share/hadoop/hdfs/lib/*,${HADOOP_MAPRED_HOME}/share/hadoop/mapreduce/*,${HADOOP_MAPRED_HOME}/share/hadoop/mapreduce/lib/*,${HADOOP_YARN_HOME}/share/hadoop/yarn/*,${HADOOP_YARN_HOME}/share/hadoop/yarn/lib/*</value>`

    </property>
</configuration>
```

![Untitled](/Imagens/Untitled%2073.png)

Por último a configuração do arquivo `workers`, nele serão inseridos os Datanodes que farão comunicação com o Namenode. Insira o nome dos Datanodes.

> **💡**: Esses nomes são das máquinas configuradas no arquivo ==hosts==.

![Untitled](/Imagens/Untitled%2074.png)

Configuração finalizada, mas antes de rodar o Hadoop, precisamos criar os diretórios onde serão salvo os logs e registros do HDFS, para isso executei os comandos:

```bash
mkdir /opt/hadoop/dfs
mkdir /opt/hadoop/dfs/data
mkdir /opt/hadoop/dfs/namespace_logs
```

Após a instalação do Hadoop,  também é necessário realizar esse processo nos Datanodes, para não precisar repetir todo processo novamente.

Farei a cópia do diretório do Hadoop no Namenode, para os Datanodes.

Para fazer a copiar do Hadoop para os Datanodes, é essencial que seja criado a pasta dele antes de receber a cópia do diretório.

> **💡**: Faça isso para todos os Datanodes.

```bash
sudo mkdir /opt/hadoop 
```

![Untitled](/Imagens/Untitled%2075.png)

Caso esteja usando o padrão que estou seguindo, é importante alterar o dono e o grupo do diretório para datalake, igualmente feito no Namenode.

```bash
sudo chown -R datalake:datalake hadoop/
```

![Untitled](/Imagens/Untitled%2076.png)

Agora posso fazer a cópia do diretório Hadoop configurado no Namenode para os Datanodes, utilizando a chave de segurança SSH.

```bash
scp -rv -i "~/.ssh/id_rsa" /opt/hadoop datalake@datanode1:/opt
scp -rv -i "~/.ssh/id_rsa" /opt/hadoop datalake@datanode2:/opt
```

Ao terminar a cópia aparecerá essa tela:

![Untitled](/Imagens/Untitled%2077.png)

Acesse a pasta `/opt/hadoop` nos Datanodes e execute o comando `ls` para verificar se os arquivos chegaram corretamente.

![Untitled](/Imagens/Untitled%2078.png)

Fim da instalação do Hadoop, agora, utilizando o Namenode formatei o HDFS

```bash
hadoop namenode -format
```

![Untitled](/Imagens/Untitled%2079.png)

<br>

<a name = "InicializacaoHadoop"></a>
<b>Inicialização do Hadoop</b>

Após a formatação do HDFS, iniciarei todos os serviços do Hadoop.

```bash
$HADOOP_HOME/sbin/start-all.sh
```

![Untitled](/Imagens/Untitled%2080.png)

Verificando através do terminal se os serviços foram iniciados corretamente nas máquinas.

```bash
jps
```

![Untitled](/Imagens/Untitled%2081.png)

![Untitled](/Imagens/Untitled%2082.png)

![Untitled](/Imagens/Untitled%2083.png)

Importando um arquivo teste e listando ele no HDFS

```bash
hdfs dfs -put alexandre.txt /
hdfs dfs -ls /
```

![Untitled](/Imagens/Untitled%2084.png)

Acessando a interface web do Hadoop utilizando meu IP do Namenode com a porta `9870`

```bash
http://192.168.1.16:9870/
```

![Untitled](/Imagens/Untitled%2085.png)

Verificando o funcionamento dos Datanodes

```bash
http://192.168.1.16:9870/dfshealth.html#tab-datanode
```

![Untitled](/Imagens/Untitled%2086.png)

Verificando visualmente os arquivos dentro do HDFS

```bash
http://192.168.1.16:9870/explorer.html#/
```

![Untitled](/Imagens/Untitled%2087.png)

Verificação do funcionamento do Yarn

```bash
http://192.168.1.16:8088/cluster
```

![Untitled](/Imagens/Untitled%2088.png)

Tudo funcionando corretamente, irei desligar o Hadoop.

```bash
$HADOOP_HOME/sbin/stop-all.sh
```

![Untitled](/Imagens/Untitled%2089.png)

<br>

<a name = "Kafka"></a>
### Instalação e configuração do Apache Kafka

> **💡**: Para a instalação do Kafka, certifique que todas as máquinas estejam iniciadas e com o Java JDK instalado.

Primeiro passo é fazer o Download do arquivo binário do Kafka:

[https://kafka.apache.org/downloads](https://kafka.apache.org/downloads)

![Untitled](/Imagens/Untitled%2090.png)

Copie o link de download do binário:

[https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.3.1.tgz](https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.3.1.tgz)

Conectado ao Namenode, no diretório `/opt/`, faça o Download do binário

```bash
sudo wget https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.3.1.tgz
```

![Untitled](/Imagens/Untitled%2091.png)

Após finalizar o Download, é necessário descompactá-lo, remover o arquivo compactado, alterar o nome e dono do arquivo.

```bash
sudo tar xzf kafka_2.13-3.3.1.tgz
sudo rm -rf kafka_2.13-3.3.1.tgz
sudo mv kafka_2.13-3.3.1/ /opt/kafka
```

![Untitled](/Imagens/Untitled%2092.png)

Adotando novamente outro padrão, estarei colocando no usuário datalake todas as instalações dos componentes Apache.

```bash
sudo chown -R datalake:datalake kafka/
```

![Untitled](/Imagens/Untitled%2093.png)

Configurando as variáveis de ambiente no arquivo `~/.bash_profile`

```bash
export KAFKA_HOME=/opt/kafka 
export PATH=$PATH:$KAFKA_HOME/bin
```

![Untitled](/Imagens/Untitled%2094.png)

No diretório `/opt/kafka/config` faça a cópia do arquivo `server.properties`

```bash
cp server.properties server1.properties
cp server.properties server2.properties
```

Agora configure os arquivos copiados.

No arquivo `server1.properties`, habilite e altere as opções:

```bash
broker.id=1
listeners=PLAINTEXT://:9093
log.dirs=/tmp/kafka-logs-1
```

No arquivo `server2.properties`, habilite e altere as opções:

```bash
broker.id=2
listeners=PLAINTEXT://:9094
log.dirs=/tmp/kafka-logs-2
```

Agora posso iniciar e testar o funcionamento do Kafka.

O primeiro passo para testar é iniciar o Zookeeper pelo diretório `/opt/kafka`

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

![Zookeeper inicializado com sucesso!](/Imagens/Untitled%2095.png)

Zookeeper inicializado com sucesso!

Abra 3 sessões de terminal e em cada terminal execute as configurações feitas dos brokers.

```bash
bin/kafka-server-start.sh config/server.properties
bin/kafka-server-start.sh config/server1.properties
bin/kafka-server-start.sh config/server2.properties
```

Verifique se a porta inicializada foi a inicializada de acordo com a configuração feita.

Inicialização com o arquivo `server.properties`

![Untitled](/Imagens/Untitled%2096.png)

Inicialização com o arquivo `server1.properties`

![Untitled](/Imagens/Untitled%2097.png)

Inicialização com o arquivo `server2.properties`

![Untitled](/Imagens/Untitled%2098.png)

Abra uma nova sessão no terminal e crie um tópico Kafka.

```bash
bin/kafka-topics.sh --create --topic BrokersTres --bootstrap-server localhost:9092 --replication-factor 3 --partitions 1
```

![Untitled](/Imagens/Untitled%2099.png)

Rode o comando abaixo para descrever o status do tópico criado.

```bash
bin/kafka-topics.sh --describe --topic BrokersTres --bootstrap-server localhost:9092
```

![Untitled](/Imagens/Untitled%20100.png)

Inicialize o producer apontando o tópico criado.

```bash
bin/kafka-console-producer.sh --topic BrokersTres --bootstrap-server localhost:9092
```

![Producer inicializado.](/Imagens/Untitled%20101.png)

Producer inicializado.

Abra uma nova sessão no terminal e inicialize o consumer apontando o tópico criado, puxando mensagens desde o início.

```bash
bin/kafka-console-consumer.sh --topic BrokersTres --from-beginning --bootstrap-server localhost:9092
```

Escreva mensagens no producer.

![Untitled](/Imagens/Untitled%20102.png)

Verifique se chegou corretamente no consumer.

![Untitled](/Imagens/Untitled%20103.png)

Tudo funcionando! Podemos encerrar todos os serviços.

```bash
bin/zookeeper-server-stop.sh config/zookeeper.properties
bin/kafka-server-stop.sh config/server.properties
bin/kafka-server-stop.sh config/server1.properties
bin/kafka-server-stop.sh config/server2.properties
```

<br>

<a name = "Nifi"></a>
### Instalação e configuração do Apache Nifi

> **💡**: Para a instalação do Nifi, certifique que as máquinas estejam com Java JDK instalado.

A instalação do Nifi será na máquina local.

Primeiro passo é fazer o Download do arquivo binário do Nifi:

![Untitled](/Imagens/Untitled%20104.png)

Copie o link de download do binário:

[https://dlcdn.apache.org/nifi/1.18.0/nifi-1.18.0-bin.zip](https://dlcdn.apache.org/nifi/1.18.0/nifi-1.18.0-bin.zip)

Conectado ao Namenode, no diretório `/opt/`, faça o Download do binário

```bash
sudo wget https://dlcdn.apache.org/nifi/1.18.0/nifi-1.18.0-bin.zip
```

![Untitled](/Imagens/Untitled%20105.png)

Após finalizar o Download, é necessário descompactá-lo, remover o arquivo compactado, alterar o nome e dono do arquivo.

```bash
sudo unzip nifi-1.18.0-bin.zip
sudo rm -rf nifi-1.18.0-bin.zip
sudo mv nifi-1.18.0/ /opt/nifi
```

Configurando as variáveis de ambiente no arquivo `~/.bash_profile`

```bash
export NIFI_HOME=/opt/nifi 
export PATH=$PATH:$NIFI_HOME/bin
```

Iniciando o Apache Nifi

> **💡**: Só irá rodar se você tiver o Java instalado em sua máquina local.

```bash
$NIFI_HOME/bin/nifi.sh start
```

![Untitled](/Imagens/Untitled%20106.png)

Crie um usuário no Nifi

```bash
$NIFI_HOME/bin/nifi.sh set-single-user-credentials <usuario> <senha>
```

Acesse

```bash
https://localhost:8443/nifi/login
```

Utilize o login criado.

![Untitled](/Imagens/Untitled%20107.png)

![Untitled](/Imagens/Untitled%20108.png)

Desligue o Nifi

```bash
$NIFI_HOME/bin/nifi.sh stop
```

Instalação do Nifi concluída.

<br>

<a name = "Spark"></a>
### Instalação e configuração do Apache Spark

> **💡**: Para a instalação do Spark, certifique que todas as máquinas estejam iniciadas e com o Java JDK instalado.

Primeiro passo é fazer o Download do arquivo binário do Spark:

[https://spark.apache.org/downloads.html](https://spark.apache.org/downloads.html)

![Untitled](/Imagens/Untitled%20109.png)

Copie o link de Download do binário:

[https://dlcdn.apache.org/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz](https://dlcdn.apache.org/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz)

![Untitled](/Imagens/Untitled%20110.png)

Abra o terminal do Namenode, vá ao diretório `/opt/` e insira o comando `wget` com o link binário do Spark:

> **💡**: Só será possível a execução do comando ==wget==, caso você tenha ele instalado.

```bash
sudo wget https://dlcdn.apache.org/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz
```

![Untitled](/Imagens/Untitled%20111.png)

Verificando, extraindo, removendo o arquivo compactado e alterando o nome do diretório.

```bash
ls -la
sudo tar xzf spark-3.3.1-bin-hadoop3.tgz
sudo rm -rf spark-3.3.1-bin-hadoop3.tgz
sudo mv spark-3.3.1-bin-hadoop3/ /opt/spark
```

![Untitled](/Imagens/Untitled%20112.png)

Adotando novamente outro padrão, estarei colocando no usuário datalake todas as instalações dos componentes Apache.

```bash
sudo chown -R datalake:datalake spark/
```

![Untitled](/Imagens/Untitled%20113.png)

Agora irei configurar as variáveis de ambiente do meu usuário datalake inserindo as seguintes configurações:

```bash
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin
```

![Untitled](/Imagens/Untitled%20114.png)

Testando seu funcionamento.

```bash
spark-shell
```

![Untitled](/Imagens/Untitled%20115.png)

Utilize o atalho `CTRL+C` para fechar o `spark-shell`

<a name = "Cluster"></a>
<b>Configurando o Spark em Multinode Cluster</b>

Entre no diretório `/opt/spark/conf` e faça uma cópia renomeada sem .template dos arquivos `spark-env.sh.template` e `workers.template`

```bash
cp spark-env.sh.template spark-env.sh
cp workers.template workers
```

Edite o arquivo `workers` e insira o IP dos Datanodes:

![Untitled](/Imagens/Untitled%20116.png)

Edite o arquivo `spark-env.sh` e insira

```bash
export SPARK_MASTER_HOST=namenode
export JAVA_HOME=/opt/jdk
```

![Untitled](/Imagens/Untitled%20117.png)

Para iniciar o Spark é necessário primeiro instalar-lo também nas outras máquinas, estarei fazendo a cópia do Namenode para os Datanodes.

Primeiramente é necessário criar o diretório do Spark nos Datanodes e alterar o dono do diretório.

```bash
sudo mkdir /opt/spark
sudo chown -R datalake:datalake /opt/spark/
```

![Untitled](/Imagens/Untitled%20118.png)

Faça a cópia do Namenode para os Datanodes.

```bash
scp -rv -i "~/.ssh/id_rsa" /opt/spark datalake@datanode1:/opt
scp -rv -i "~/.ssh/id_rsa" /opt/spark datalake@datanode2:/opt
```

Verifique se chegou corretamente nos Datanodes.

![Untitled](/Imagens/Untitled%20119.png)

Agora podemos rodar o Multinode pelo Namenode

```bash
$SPARK_HOME/sbin/start-all.sh
```

![Untitled](/Imagens/Untitled%20120.png)

Verificando em todas as máquinas se os serviços foram iniciados corretamente

```bash
jps
```

Namenode

![Untitled](/Imagens/Untitled%20121.png)

Datanode1

![Untitled](/Imagens/Untitled%20122.png)

Datanode2

![Untitled](/Imagens/Untitled%20123.png)

Inicie o `spark-shell`

```bash
spark-shell --master spark://namenode:7077
```

![Untitled](/Imagens/Untitled%20124.png)

Utilize o atalho `CTRL+C` para fechar o `spark-shell`

Verifique se todos os Datanodes estão sincronizados.

```bash
http://192.168.1.9:4040
```

![Untitled](/Imagens/Untitled%20125.png)

Também é possível verificar através do endereço:

```bash
http://192.168.1.9:8080/
```

![Untitled](/Imagens/Untitled%20126.png)

Parando todos os serviços

```bash
$SPARK_HOME/sbin/stop-all.sh
```

Instalação do Spark concluída.

<br>

<a name = "Hive"></a>
### Instalação e configuração do Apache Hive

> **💡**: Para a instalação do Hive, certifique que todas as máquinas estejam iniciadas e com o Java JDK instalado.

Primeiro passo é fazer o Download do arquivo binário do Hive:

[https://dlcdn.apache.org/hive/](https://dlcdn.apache.org/hive/)

![Untitled](/Imagens/Untitled%20127.png)

Escolha a versão e copie o link de Download do binário:

[https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz](https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz)

![Untitled](/Imagens/Untitled%20128.png)

Abra o terminal do Namenode, vá ao diretório `/opt/` e insira o comando `wget` com o link binário do Hive:

> **💡**: Só será possível a execução do comando ==wget==, caso você tenha ele instalado.

```bash
sudo wget https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
```

![Untitled](/Imagens/Untitled%20129.png)

Verificando, extraindo, removendo o arquivo compactado e alterando o nome do diretório.

```bash
ls -la
sudo tar xzf apache-hive-3.1.3-bin.tar.gz 
sudo rm -rf apache-hive-3.1.3-bin.tar.gz 
sudo mv apache-hive-3.1.3-bin/ /opt/hive
```

![Untitled](/Imagens/Untitled%20130.png)

Estarei colocando no usuário datalake todas as instalações dos componentes Apache.

```bash
sudo chown -R datalake:datalake hive/
```

![Untitled](/Imagens/Untitled%20131.png)

Exportando as variáveis de ambiente:

```bash
export HIVE_HOME=/opt/hive
export PATH=$PATH:$HIVE_HOME/bin
```

![Untitled](/Imagens/Untitled%20132.png)

Verifique a versão do Hive

```bash
hive --version
```

![Untitled](/Imagens/Untitled%20133.png)

Antes de inicializar é necessário ter um banco de dados relacional para armazenar o Metastore do Hive.

<br>

<a name = "Oracle"></a>
<b>Configuração do Metastore com Banco de Dados Oracle</b>

É necessário o Download do conector JDBC do Oracle.

[https://www.oracle.com/br/database/technologies/appdev/jdbc-downloads.html](https://www.oracle.com/br/database/technologies/appdev/jdbc-downloads.html)

![Untitled](/Imagens/Untitled%20134.png)

Copie o link e faça o Download no diretório `/opt/hive/lib/`

```bash
sudo wget https://download.oracle.com/otn-pub/otn_software/jdbc/217/ojdbc8.jar
```

![Untitled](/Imagens/Untitled%20135.png)

Altere o dono e grupo do arquivo para datalake

```bash
sudo chown -R datalake:datalake ojdbc8.jar
```

Antes de qualquer passo é necessário iniciar o HDFS.

```bash
$HADOOP_HOME/sbin/start-dfs.sh
```

![Untitled](/Imagens/Untitled%20136.png)

Iniciando o banco de dados Oracle

![Untitled](/Imagens/Untitled%20137.png)

Utilizarei o SQL Developer para se conectar ao Oracle

Se conectando ao usuário sys

![Untitled](/Imagens/Untitled%20138.png)

Criando um usuário no banco de dados e dando privilégios:

![Untitled](/Imagens/Untitled%20139.png)

Testando a conexão

![Untitled](/Imagens/Untitled%20140.png)

Agora rode o script que é disponibilizado pelo Hive no diretório 

```bash
/opt/hive/scripts/metastore/upgrade/oracle
```

Copie o conteúdo dentro do arquivo `hive-schema-3.1.0.oracle.sql` e rode no SQL Developer.

![Untitled](/Imagens/Untitled%20141.png)

Agora é necessário configurar o Hive para se conectar com o banco Oracle

No diretório `/opt/hive/conf` crie o arquivo `hive-site.xml`

```bash
vi hive-site.xml
```

Dentro do arquivo insira as configurações.

```html
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:oracle:thin:@//192.168.1.11/ORCL12C</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>oracle.jdbc.OracleDriver</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>hive</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>hive</value>
    </property>
</configuration>
```

![Untitled](/Imagens/Untitled%20142.png)

Rode o comando para criar o schema:

> **💡**: Para funcionar corretamente é necessário configurar o arquivo ==tnsnames.ora== do banco de dados Oracle.

```bash
/opt/hive/bin/schematool -dbType oracle -initSchema
```

Agora é só rodar o comando Hive.

> **💡**: O comando ==hive== só vai funcionar se as variáveis de ambiente estiverem configuradas corretamente.

![Untitled](/Imagens/Untitled%20143.png)

Instalação concluída.

<br>
<a name = "MySQL"></a>
<b>Configuração do Metastore com Banco de Dados MySQL</b>

Baixar o conector JDBC do MySQL

[https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/)

![Untitled](/Imagens/Untitled%20144.png)

Utilize o comando wget para baixar no diretório `/opt/hive/lib`

```bash
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.0.31.zip
```

![Untitled](/Imagens/Untitled%20145.png)

Após baixar é hora de extrair:

```bash
sudo unzip mysql-connector-j-8.0.31.zip
```

![Untitled](/Imagens/Untitled%20146.png)

Removendo o arquivo baixado.

```bash
sudo rm -rf mysql-connector-j-8.0.31.zip
```

Entre no diretório `mysql-connector-j-8.0.31`

```bash
cd /opt/hive/lib/mysql-connector-j-8.0.31
```

Mova o `mysql-connector-j-8.0.31.jar` para o `/opt/hive/lib/`

```bash
sudo mv mysql-connector-j-8.0.31.jar /opt/hive/lib
```

Altere o dono e grupo do arquivo `mysql-connector-j-8.0.31.jar` 

```bash
sudo chown -R datalake:datalake mysql-connector-j-8.0.31.jar
```

Agora crie um banco de dados no MySQL

Inicie o MySQL

```bash
mysql -u root -p
```

![Untitled](/Imagens/Untitled%20147.png)

```sql
CREATE DATABASE METASTORE;
```

![Untitled](/Imagens/Untitled%20148.png)

Entre no banco criado.

```sql
USE METASTORE;
```

![Untitled](/Imagens/Untitled%20149.png)

Agora crie o schema do Hive

```sql
source /opt/hive/scripts/metastore/upgrade/mysql/hive-schema-3.1.0.mysql.sql
```

![Untitled](/Imagens/Untitled%20150.png)

Criação de um usuário para a conexão do Hive.

```sql
CREATE USER 'hive' IDENTIFIED BY 'eYsfh$99N6';
```

![Untitled](/Imagens/Untitled%20151.png)

Concessão de privilégios para o usuário `hive`

```sql
GRANT ALL PRIVILEGES ON *.* TO hive;
flush privileges;
```

![Untitled](/Imagens/Untitled%20152.png)

Agora é hora de configurar o Hive.

Entre no diretório `/opt/hive/conf`,crie o arquivo `hive-site.xml` e insira as configurações.

```html
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://localhost/metastore?createDatabaseIfNotExist=true</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>hive</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>eYsfh$99N6</value>
    </property>
</configuration>
```

Tudo configurado, agora podemos rodar o Hive, para isso é necessário antes rodar o HDFS e o Yarn.

```bash
$HADOOP_HOME/sbin/start-dfs.sh
$HADOOP_HOME/sbin/yarn-dfs.sh
```

![Untitled](/Imagens/Untitled%20153.png)

Antes de rodar o hive é necessário tirar o HDFS do modo Safe:

```bash
hdfs dfsadmin -safemode leave
```

Rode o comando para iniciar o schema criado no MySQL.

```bash
schematool -dbType mysql -initSchema
```

![Untitled](/Imagens/Untitled%20154.png)

Agora rodamos o comando `Hive`

![Untitled](/Imagens/Untitled%20155.png)

Concluído a instalação

<br>
<a name = "HiveTest"></a>
<b>Testando o Hive</b>

Para testar o Hive primeiramente irei criar uma tabela nele.

```sql
CREATE TABLE datalake (COL1 INT);
```

![Untitled](/Imagens/Untitled%20156.png)

Verificando o Metastore pelo banco de dados MySQL. 

```sql
USE METASTORE;
SELECT * FROM TBLS;
```

![Untitled](/Imagens/Untitled%20157.png)

Metastore da tabela Hive (Datalake) criada com sucesso!

Para esse teste vou incluir arquivos `.csv` no meu HDFS. Estarei utilizando como base um arquivo `.csv` da minha autoria e o repartir em 3 arquivos.

![Untitled](/Imagens/Untitled%20158.png)

Criando a pasta cliente e movendo os arquivos `.csv` para dentro dela.

```bash
hdfs dfs -mkdir /cliente
hdfs dfs -mv /cliente1.csv /cliente
hdfs dfs -mv /cliente2.csv /cliente
hdfs dfs -mv /cliente3.csv /cliente
hdfs dfs -ls /cliente
```

![Untitled](/Imagens/Untitled%20159.png)

Estruturando arquivos `.csv` para rodar no Hive.

```sql
CREATE EXTERNAL TABLE Cliente (
CPF STRING,
NOME STRING,
SOBRENOME STRING,
DATA_NASCIMENTO STRING,
ID_ENDERECO STRING,
EMAIL STRING,
CONTATO STRING,
SALARIO STRING,
DATA_INICIO STRING,
DATA_FIM STRING,
ID_FUNCIONARIO STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
location '/cliente/';
```

![Untitled](/Imagens/Untitled%20160.png)

Executando uma query

```sql
SELECT * FROM CLIENTE;
```

![Untitled](/Imagens/Untitled%20161.png)

Concluído o teste com o Hive!

Após todas essas instalações, o Data Lake está pronto para uso.