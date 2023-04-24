### Sumário
+ [Configurações do sistema operacional](#Linux)
+ [Atualização](#Atualizacao)
+ [Arquivo sudoers](#Sudoers)
+ [Arquivo hosts](#Hosts)
+ [Arquivo sshd_config](#SSHD)
+ [SSH](#SSH)

<a name = "Linux"></a>

### Configuração inicial do Linux

Para começar a configurar as máquinas, estarei me conectando através do SSH da minha máquina local nas máquinas virtuais no terminal.

Como a minha máquina principal também é Linux, eu consigo diretamente pelo terminal utilizar o comando SSH para me conectar nas máquinas, para isso, antes é necessário ligar o Namenode e os Datanodes, no meu caso 2 Datanodes.

> **💡**: Para usuários de Windows: Se você tentar se conectar pelo PowerShell do Windows, você não conseguirá, para solucionar isso baixe o Putty e faça a conexão diretamente dele.

Abra o Virtual Box

Selecione o Namenode e os Datanodes e clique em:

- Iniciar (T)

![Untitled](/Imagens/Untitled%2032.png)

Abrirá neste menu.

Selecione a primeira opção apertando o ‘Enter’ do teclado.

![Untitled](/Imagens/Untitled%2033.png)

Após iniciar e aguardar os logs de inicialização, faça o login criado na instalação, realizado na  etapa anterior.

![Untitled](/Imagens/Untitled%2034.png)

Repita esse processo para todos os Datanodes.

Datanode1:

![Untitled](/Imagens/Untitled%2035.png)

Datanode2: 

![Untitled](/Imagens/Untitled%2036.png)

Após ligar e logar em todas máquinas, precisamos do IP de cada uma delas para realizar a conexão via SSH. Entre em cada máquina e colete o IP delas através do comando:

```bash
ip a 
```

![Untitled](/Imagens/Untitled%2037.png)

IP das minhas máquinas:

```bash
Namenode: 192.168.1.16
Datanode1: 192.168.1.14
Datanode2: 192.168.1.15
```

> **💡**: Vale ressaltar que esses IP’s não serão necessariamente iguais aos meus (mesmo podendo ser também), por isso é importante anotar eles.

Após as coletas, abra o Terminal da sua máquina local e insira o comando:

```bash
ssh usuário@ipdamaquina 
```

Depois aparecerá este aviso:

`Are you sure you want to continue connecting (yes/no/[fingerprint])?`

Digite `yes`. Depois será solicitado a senha do usuário, digite ela e estará conectado.

> **💡**: Usuário criado na instalação do CentOS

Conectando com a minha máquina no Namenode:

> **💡**: Para usuários de Windows: Momento de usar o Putty e fazer a conexão via SSH.

```bash
ssh datalake@192.168.1.16
```

![Neste momento já estou conectado ao meu Namenode pelo SSH na minha máquina local.](/Imagens/Untitled%2038.png)

Neste momento já estou conectado ao meu Namenode pelo SSH na minha máquina local.

Repita esse processo novamente para todos os Datanodes abrindo mais janelas no terminal.

Conexão com o Datanode1:

```bash
ssh datalake@192.168.1.14
```

![Untitled](/Imagens/Untitled%2039.png)

Conexão com o Datanode2:

```bash
ssh datalake@192.168.1.15
```

![Untitled](/Imagens/Untitled%2040.png)

Agora, estamos oficialmente 100% prontos para as configurações do Linux.

<br>

<a name = "Sudoers"></a>
<b> Configuração do Sudoers </b>

> **💡**: É necessário realizar esse passo para todas as máquinas.

Para poder realizar os comandos de Root no usuário datalake, é necessário configurar o arquivo sudoers. 

Primeiro passo, entre no root:

```bash
su - 
```

Edite o arquivo sudoers:

```bash
vi /etc/sudoers
```

Insira dentro do arquivo o nome do usuário datalake.

```bash
datalake    ALL=(ALL)    ALL
```

![Untitled](/Imagens/Untitled%2041.png)

Salve o arquivo e saia do perfil Root, executando o comando:

```bash
exit
```

<br>

<a name = "Atualizacao"></a>
<b> Atualizando o sistema</b>

> **💡**: É necessário realizar esse passo para todas as máquinas.

Após a configuração do arquivo `sudoers` e acesso do Linux com o usuário datalake vou verificar se possui atualizações e se sim, realizar elas.

```bash
sudo yum update
```

Quando terminar a atualização ele mostrará uma mensagem de concluído na tela.

![Untitled](/Imagens/Untitled%2042.png)

<br>
<a name = "Hosts"></a>
<b>Configurando o arquivo hosts</b>

Para as máquinas poderem se comunicar entre si, temos que configurar as chaves de segurança SSH, para esta tarefa precisamos primeiro configurar o arquivo hosts das nossas máquinas.

Para modificar o arquivo hosts, use o comando:

```bash
sudo vi /etc/hosts 
```

Informe para o arquivo hosts qual são os IP’s das máquinas que ele fará conexão e ao seu lado coloque o nome indicativo de qual máquina está apontando o IP.

```bash
192.168.1.14   datanode1
192.168.1.15   datanode2
```

![Untitled](/Imagens/Untitled%2043.png)

Repita esse processo para todas as máquinas modificando os IP’s e nomes das máquinas.

![Untitled](/Imagens/Untitled%2044.png)

Depois de configurado, devemos testar para ver se as máquinas estão se comunicando entre-si, só rodar o comando em qualquer máquina:

```bash
ping datanode1 
ping datanode2
```

![Caso o ping esteja retornando os milissegundos entre as conexões, significa que foi configurado corretamente.](/Imagens/Untitled%2045.png)

Caso o ping esteja retornando os milissegundos entre as conexões, significa que foi configurado corretamente.

<br>

<a name = "SSHD"></a>
<b>Configurando o arquivo sshd_config</b>

Para habilitar a conexão via SSH entre máquinas é necessário configurar o arquivo sshd_config, nele estão as configurações de conexão do SSH.

> **💡**: É necessário realizar esse passo para todas as máquinas.

Para editar este arquivo, rodamos o comando:

```bash
sudo vi /etc/ssh/sshd_config
```

Após abrir o editor no arquivo, é necessário remover o comentário dos comandos:

```bash
Port 22
ListenAddress 0.0.0.0
ListenAddress ::
PubkeyAuthentication yes
```

![Untitled](/Imagens/Untitled%2046.png)

Por último reinicie o serviço do sshd para alterar as configurações feitas:

```bash
sudo systemctl restart sshd
```

Pronto! Arquivo `sshd` devidamente configurado. 

<br>

<a name = "SSH"></a>
<b>Configurando as chaves de segurança SSH</b>

Após a configuração do arquivo hosts e arquivo sshd, agora é hora de configurar o SSH.

A configuração da chave SSH fará com que as máquinas se comuniquem sem precisarem de senhas no momento em que algo for feito entre as máquinas.

> **💡**: É necessário realizar todos passos seguintes apenas no Namenode.

Para gerar a chave de SSH, precisamos estar em nosso Namenode e rodar o comando:

```bash
ssh-keygen
```

Caso queira fazer alguma modificação na chave, você tem essa opção, no meu caso vou deixar tudo como padrão, seguindo apertando ‘Enter’ para gerar a chave.

> **💡**: Vale ressaltar que essa chave foi criada no usuário datalake, cada chave é gerada em cada usuário. Caso tente ver essa chave em outro usuário da mesma máquina, não conseguirá.

![Untitled](/Imagens/Untitled%2047.png)

Para verificar a criação da chave utilize o comando:

```bash
cat ~/.ssh/nomedachave
```

No meu caso a chave criada se chama id_rsa (padrão)

comando: 

```bash
cat ~/.ssh/id_rsa
```

![Untitled](/Imagens/Untitled%2048.png)

Chave gerada! É necessário copiar essa chave para o próprio usuário datalake

```bash
ssh-copy-id -i ~/.ssh/id_rsa datalake@namenode
```

Essa chave também precisa ser copiada para as outras máquinas fazendo com que elas se comuniquem.

```bash
ssh-copy-id -i ~/.ssh/chave usuario@datanode
```

Cópia da chave para os Datanodes.

```bash
ssh-copy-id -i ~/.ssh/id_rsa datalake@datanode1
ssh-copy-id -i ~/.ssh/id_rsa datalake@datanode2
```

![Será solicitado a confirmação da conexão, só responder com `yes` e também será solicitado a senha do usuário.](/Imagens/Untitled%2049.png)

Será solicitado a confirmação da conexão, só responder com `yes` e também será solicitado a senha do usuário.

> **💡**: Só foi possível utilizar o nome do host Datanode1 por conta da configuração do arquivos hosts, caso o arquivos hosts não estivesse configurado, o nome do host poderia ser substituído pelo IP do host.

Repita esse comando para todos os Datanodes.

> **💡**: Cópia da chave para o Datanode2.

![Untitled](/Imagens/Untitled%2050.png)

Para verificar o funcionamento da chave, vou conectar através do SSH nos Datanodes.

```bash
ssh datalake@datanode1 -i ~/.ssh/id_rsa
ssh datalake@datanode2 -i ~/.ssh/id_rsa
```

![Conexão feita com sucesso, sem solicitar a senha, perceba que agora possui duas janelas de Datanode1. Verifique se os outros Datanodes também estão se conectando via SSH sem pedir senha.](/Imagens/Untitled%2051.png)

Conexão feita com sucesso, sem solicitar a senha, perceba que agora possui duas janelas de Datanode1. Verifique se os outros Datanodes também estão se conectando via SSH sem pedir senha.

Após isso podemos sair da conexão e voltar para o Namenode, utilizando o comando:

```bash
exit
```

Chave configurada, tudo oficialmente pronto para o início da instalação do Hadoop.