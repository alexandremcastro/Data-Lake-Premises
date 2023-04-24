### Sumário
+ [Preparando as máquinas](#Maquinas)
+ [Criação do Namenode](#Namenode)
+ [Criação do Datanode](#Datanode)

<a name = "Maquinas"></a>
### Preparando as máquinas

Antes de qualquer passo, para este projeto, estarei utilizando o Virtualbox da Oracle para a virtualização das máquinas, mas não é obrigatório a utilização deste, podendo ser qualquer outro virtualizador, desde de que seja possível a configuração de rede das máquinas.


Iniciar o Virtualbox e selecionar a opção:

- Novo

![Captura de tela de 2022-11-09 10-38-04.png](Imagens/Captura_de_tela_de_2022-11-09_10-38-04.png)

Segue o modelo de criação das diferentes máquinas:

<br>

<a name = "Namenode"></a>
<b>Criação do Namenode</b>

<aside>
💡 É obrigatório a criação do Namenode, por mais que um dia não vá utilizar nenhum Datanode.

</aside>

Escolha um nome para a sua máquina que será o Namenode, seguindo essas configurações de tipo e versão, pois estaremos utilizando o CentOS como sistema operacional.

Selecione a opção:

- Próximo (N) >

![Untitled](Imagens/Untitled.png)

Em seguida selecione a quantidade de memória que será alocada a máquina, para o Namenode é importante que seja um pouco mais forte do que os Datanodes, já que ele que será o orquestrador do Hadoop, neste caso estarei alocando **4GB** de memória RAM.

Selecione a opção:

- Próximo (N) >

![Untitled](Imagens/Untitled%201.png)

Criação do disco rígido virtual para armazenamento dos dados da máquina.

Selecione a opção:

- Criar um novo disco rígido virtual agora
- Próximo (N) >

![Untitled](Imagens/Untitled%202.png)

Selecione a opção:

- VDI (VirtualBOX Disk Image)
- Próximo (N) >

![Untitled](Imagens/Untitled%203.png)

Selecione a opção:

- Dinamicamente alocado

<aside>
💡 Importante que o disco seja dinamicamente alocado para não ocupar todo o espaço reservado em sua máquina física.

</aside>

- Próximo (N) >

![Untitled](Imagens/Untitled%204.png)

Selecione a quantidade que será alocada dinamicamente no disco virtual, recomendo no mínimo **10GB**, no entanto estarei colocando **20GB** para ter uma margem a mais mantendo o diretório padrão de onde será armazenado o disco virtual.

Selecione a opção:

- Criar

![Untitled](Imagens/Untitled%205.png)

Após isso, foi criado o Namenode, agora temos que alterar as configurações de rede, para as máquinas poderem se comunicarem entre si.

Selecione a opção:

- Configurações

![Untitled](Imagens/Untitled%206.png)

Abrirá a tela de configurações, vá na aba Rede.

Clique em:

- Conectado e selecione a opção ‘Placa em modo Bridge’
- Clique em OK

![Untitled](Imagens/Untitled%207.png)

Seguindo esses passos, o Namenode agora está habilitado a ter seu IP próprio para realizar a configuração de conexão entre os clusters.

<br>

<a name = "Datanode"></a>
<b>Criação do Datanode</b>

Para sua criação, seguiremos praticamente os mesmos passos da criação do Namenode, com apenas duas alterações.

A primeira alteração será no nome da máquina, neste caso será o Datanode1

<aside>
💡 Estarei utilizando 2 Datanodes para a formação do meu datalake, mas não é obrigatório possuir Datanodes, ou apenas 2 Datanodes. Podem ser criados infinitos Datanodes, desde que você tenha capacidade computacional para isso.

</aside>

![Untitled](Imagens/Untitled%208.png)

A segunda alteração será na quantidade de memória, alocarei apenas **2GB**, porque como citei na criação do Namenode, os Datanodes podem ter menos poder computacional.

![Untitled](Imagens/Untitled%209.png)

<br>
