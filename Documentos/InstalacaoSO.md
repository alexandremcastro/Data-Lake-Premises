### Sumário
+ [Instalação do Linux (CentOS)](#CentOS)
+ [Instalação do CentOS no Namenode](#CentOSNamenode)
+ [Instalação do CentOS no Datanode](#CentOSDatanode)

<a name = "CentOS"></a>

### Instalação do Linux (CentOS)

Para a instalação do sistema operacional, é necessário baixar a ISO dele, neste caso estarei utilizando a versão Minimal do CentOS, para baixo consumo de recursos e ganho de performance nos clusters.

Acesse a página de Download do CentOS

[https://www.centos.org/download/](https://www.centos.org/download/)

Estarei utilizando a versão x86_64

Selecione a opção:

- x86_64

<br>
<aside>
💡 Também é possível escolher outras versões caso a x86_64 não seja compatível com seu virtualizador ou máquina.
</aside>
<br>

![Untitled](/Imagens/Untitled%2010.png)

Selecione um dos mirrors de Download, estarei selecionando o [primeiro](http://mirror.uepg.br/centos/7.9.2009/isos/x86_64/)

![Untitled](/Imagens/Untitled%2011.png)

Selecione a opção:

- CentOS-7-x86_64-Minimal-2009.iso

![Untitled](/Imagens/Untitled%2012.png)

Após realizar o Download é hora de instalar o CentOS nas máquinas que foram criadas.

<br>

<a name = "CentOSNamenode"></a>
<b>Instalação do CentOS no Namenode</b>

Abra o Virtual Box, clique no Namenode.

<aside>
💡 Máquina criada na etapa anterior.

</aside>

Selecione a opção:

- Iniciar (T)

![Untitled](/Imagens/Untitled%2013.png)

Abra essa janela, clique no símbolo da pasta ao lado do campo de seleção.

![Untitled](/Imagens/Untitled%2014.png)

Selecione o diretório da ISO do CentOS (CentOS-7-x86_64-Minimal-2009.iso).

Clique em:

- Acrescentar

![Untitled](/Imagens/Untitled%2015.png)

Após selecionar a ISO clique em:

- Iniciar

![Untitled](/Imagens/Untitled%2016.png)

Após iniciar, selecione a opção:

- Install CentOS 7

![Untitled](/Imagens/Untitled%2017.png)

Após as telas de logs, aparecerá o instalador do CentOS.

Nesta tela você configura as linguagens do SO, no meu caso deixarei no padrão (English).

Selecione a opção:

- Continue

![Untitled](/Imagens/Untitled%2018.png)

Agora vou configurar as opções de região, data e hora.

Selecione a opção:

- DATE & TIME

![Untitled](/Imagens/Untitled%2019.png)

Marque a região mais próxima de onde você se localiza

Selecione a opção:

- Done

![Untitled](/Imagens/Untitled%2020.png)

Indicando o destino de onde será instalado o SO.

Desça a tela e selecione a opção:

- INSTALLATION DESTINATION

![Untitled](/Imagens/Untitled%2021.png)

Marque o disco virtual criado na etapa anterior.

Selecione a opção:

- Done

![Untitled](/Imagens/Untitled%2022.png)

Configuração de rede e nome do Host

Selecione a opção:

- NETWORK & HOST NAME

![Untitled](/Imagens/Untitled%2023.png)

Marque a caixinha como ON embaixo do ‘botão Help!’, para habilitar a internet na máquina.

Altere o Host name para o nome da sua máquina criada na etapa anterior e clique em:

- Apply
- Done

![Untitled](/Imagens/Untitled%2024.png)

Começar a instalação do SO.

Clique em:

- Begin Installation

![Untitled](/Imagens/Untitled%2025.png)

Configuração da senha do usuário Root

Clique em:

- ROOT PASSWORD

![Untitled](/Imagens/Untitled%2026.png)

Definindo uma senha para o Root

Insira uma senha segura e clique em:

- Done

![Untitled](/Imagens/Untitled%2027.png)

Criação de um usuário

Clique em:

- USER CREATION

![Untitled](/Imagens/Untitled%2028.png)

Preencha o nome do usuário e crie uma senha segura para ele, depois clique em:

- Done

![Untitled](/Imagens/Untitled%2029.png)

Após toda a configuração, espere a barra de instalação completar e aperte o botão:

- Reboot

![Untitled](/Imagens/Untitled%2030.png)

<a name = "CentOSDatanode"></a>
<b> Instalação do CentOS no Datanode </b>

A instalação no Datanode é praticamente igual ao do Namenode, porém é necessário alterar o Host name para o nome da sua máquina Datanode, repita esse processo para todos os Datanodes.

![Untitled](/Imagens/Untitled%2031.png)