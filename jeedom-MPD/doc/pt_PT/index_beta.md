# Plugin MPD

Plugin que permite controlar um leitor MPD.

O Music Player Daemon, ou MPD, é um reprodutor de áudio livre que permite o acesso remoto a partir de outro computador. Funciona em segundo plano em muitos servidores multimédia, como o Moodeaudio, o Volumio, etc.

O MPD permite reproduzir os ficheiros de áudio (= Song) que se encontram na sua fila (= Queue). Esta é alimentada pelas listas de reprodução (as listas de reprodução não são geridas pelo plugin).

O plugin permite executar as funções básicas (carregamento de listas de reprodução, reprodução, volume, etc.) a partir do Jeedom. O plugin utiliza o utilitário mpc para executar os comandos no servidor MPD, quer este se encontre localmente ou remotamente. O pacote mpc é instalado aquando da ativação do plugin (ligação para o GitHub <https://github.com/MusicPlayerDaemon/mpc>).

# Instalação e configuração do servidor MPD

Para que o plugin funcione corretamente, é necessário que o servidor MPD esteja operacional.

Este é, na maioria das vezes, instalado de forma transparente pelo servidor multimédia correspondente (Volumio, Moodeaudio, ...).

Por predefinição, o servidor MPD aguarda comandos na porta 6600. O acesso ao mesmo pode ser controlado por uma palavra-passe.

# Configuração do plugin

Depois de instalar o plugin, é necessário ativá-lo.

É possível ativar o nível de registo «Debug» para acompanhar a atividade do plugin e identificar eventuais problemas.

# Configuração dos equipamentos

A configuração dos equipamentos está acessível a partir do menu do plugin (menu Plugins, Multimédia e, em seguida, MPD).

Clique em «Adicionar» para configurar um novo controlador MPD.

![MPD_Equipamento](../images/MPD_Equipement.png)

Indique a configuração do MPD:

-   **Nome**: nome do MPD
-   **Objeto pai**: indica o objeto pai ao qual o equipamento pertence
-   **Categoria**: indica a categoria Jeedom do equipamento; por predefinição, «Multimédia»
-   **Ativar**: permite ativar o equipamento
-   **Visível**: torna-o visível no painel de controlo
-   **Endereço IP**: IP do servidor MPD; deixar em branco se estiver instalado no servidor Jeedom
-   **Porta**: porta de escuta do servidor MPD; deixar em branco se for o valor predefinido
-   **Palavra-passe**: palavra-passe para aceder ao servidor MPD

Os botões seguintes permitem as seguintes funções:

-   **Testar a ligação ao MPD**: permite verificar se os parâmetros de ligação estão corretos (não se esqueça de guardar a configuração antes de clicar no botão).
-   **Gerar comandos**: permite gerar os comandos necessários para controlar o leitor (útil apenas se tiver sido eliminado um dos comandos).

# Comandos associados aos equipamentos

![MPD_Comandos](../images/MPD_Commandes.png)

Os comandos básicos são gerados aquando da criação do equipamento.

Para cada comando do tipo «ação», o campo «Comando» (armazenado no LogicalID do comando Jeedom) indica o comando transmitido ao utilitário mpc. Consulte a documentação do mpc para obter mais informações ( <https://www.musicpd.org/doc/mpc/html/> ).

![MPD_Comandos_Adicionar](../images/MPD_Commandes_Ajout.png)

O comando «Criar um comando» permite adicionar uma ação, por exemplo, para criar um atalho para reproduzir uma estação de rádio. Para tal, pode utilizar-se o comando «playsong», que será transformado em «play» seguido do número da música na fila.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

A apresentação criada por predefinição permite executar as funções básicas. Repare no botão «refresh» (no canto superior direito do widget), que permite atualizar o estado do leitor MPD (listas de reprodução, música a tocar, etc.). Ao selecionar uma lista de reprodução, a fila do MPD é inicializada com as músicas correspondentes. A seleção de uma música permite reproduzi-la.

![MPD_Equipamento_Disposição](../images/MPD_Equipement_Disposition.png)

A apresentação é feita através da função «Disposição do equipamento» (em «Configuração avançada»).

![MPD_Widget_Favoritos](../images/MPD_Widget_Favoris.png)

Ao alterar a apresentação, é possível adicionar atalhos.

# Controlo do leitor de áudio a partir de um Mi Cube

![MPD_micube](../images/MPD_micube.png)

Ao utilizar os cenários, é possível controlar o leitor de áudio sem recorrer à interface do Jeedom, a partir de um dispositivo de comando como, por exemplo, o Mi Cube da Xiaomi.

![MPD_micube_song](../images/MPD_micube_song.png)

O cenário acima, ativado quando o estado de #[Nenhum][Cubo][lado]# muda, permite mudar a estação de rádio, alterando o lado do Mi Cube.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

O cenário acima, ativado quando o estado de #[Nenhum][Cube][Botão]# muda, permite parar e reiniciar a música ao agitar o Mi Cube.

# Plugin MPD

Plugin que permite controlar um leitor MPD.

O Music Player Daemon, ou MPD, é um reprodutor de áudio livre que permite o acesso remoto a partir de outro computador. Funciona em segundo plano em muitos servidores multimédia, como o Moodeaudio, o Volumio, etc.

O MPD permite reproduzir os ficheiros de áudio (= Song) que se encontram na sua fila (= Queue). Esta é alimentada pelas listas de reprodução (as listas de reprodução não são geridas pelo plugin).

O plugin permite executar as funções básicas (carregamento de listas de reprodução, reprodução, volume, etc.) a partir do Jeedom. O plugin utiliza o utilitário mpc para executar os comandos no servidor MPD, quer este se encontre localmente ou remotamente. O pacote mpc é instalado aquando da ativação do plugin (ligação para o GitHub <https://github.com/MusicPlayerDaemon/mpc>).

# Instalação e configuração do servidor MPD

Para que o plugin funcione corretamente, é necessário que o servidor MPD esteja operacional.

Este é, na maioria das vezes, instalado de forma transparente pelo servidor multimédia correspondente (Volumio, Moodeaudio, ...).

Por predefinição, o servidor MPD aguarda comandos na porta 6600. O acesso ao mesmo pode ser controlado por uma palavra-passe.

# Configuração do plugin

Depois de instalar o plugin, é necessário ativá-lo.

É possível ativar o nível de registo «Debug» para acompanhar a atividade do plugin e identificar eventuais problemas.

# Configuração dos equipamentos

A configuração dos equipamentos está acessível a partir do menu do plugin (menu Plugins, Multimédia e, em seguida, MPD).

Clique em «Adicionar» para configurar um novo controlador MPD.

![MPD_Equipamento](../images/MPD_Equipement.png)

Indique a configuração do MPD:

-   **Nome**: nome do MPD
-   **Objeto pai**: indica o objeto pai ao qual o equipamento pertence
-   **Categoria**: indica a categoria Jeedom do equipamento; por predefinição, «Multimédia»
-   **Ativar**: permite ativar o equipamento
-   **Visível**: torna-o visível no painel de controlo
-   **Endereço IP**: IP do servidor MPD; deixar em branco se estiver instalado no servidor Jeedom
-   **Porta**: porta de escuta do servidor MPD; deixar em branco se for o valor predefinido
-   **Palavra-passe**: palavra-passe para aceder ao servidor MPD

Os botões seguintes permitem as seguintes funções:

-   **Testar a ligação ao MPD**: permite verificar se os parâmetros de ligação estão corretos (não se esqueça de guardar a configuração antes de clicar no botão).
-   **Gerar comandos**: permite gerar os comandos necessários para controlar o leitor (útil apenas se tiver sido eliminado um dos comandos).

# Comandos associados aos equipamentos

![MPD_Comandos](../images/MPD_Commandes.png)

Os comandos básicos são gerados aquando da criação do equipamento.

Para cada comando do tipo «ação», o campo «Comando» (armazenado no LogicalID do comando Jeedom) indica o comando transmitido ao utilitário mpc. Consulte a documentação do mpc para obter mais informações ( <https://www.musicpd.org/doc/mpc/html/> ).

![MPD_Comandos_Adicionar](../images/MPD_Commandes_Ajout.png)

O comando «Criar um comando» permite adicionar uma ação, por exemplo, para criar um atalho para reproduzir uma estação de rádio. Para tal, pode utilizar-se o comando «playsong», que será transformado em «play» seguido do número da música na fila.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

A apresentação criada por predefinição permite executar as funções básicas. Repare no botão «refresh» (no canto superior direito do widget), que permite atualizar o estado do leitor MPD (listas de reprodução, música a tocar, etc.). Ao selecionar uma lista de reprodução, a fila do MPD é inicializada com as músicas correspondentes. A seleção de uma música permite reproduzi-la.

![MPD_Equipamento_Disposição](../images/MPD_Equipement_Disposition.png)

A apresentação é feita através da função «Disposição do equipamento» (em «Configuração avançada»).

![MPD_Widget_Favoritos](../images/MPD_Widget_Favoris.png)

Ao alterar a apresentação, é possível adicionar atalhos.

# Controlo do leitor de áudio a partir de um Mi Cube

![MPD_micube](../images/MPD_micube.png)

Ao utilizar os cenários, é possível controlar o leitor de áudio sem recorrer à interface do Jeedom, a partir de um dispositivo de comando como, por exemplo, o Mi Cube da Xiaomi.

![MPD_micube_song](../images/MPD_micube_song.png)

O cenário acima, ativado quando o estado de #[Nenhum][Cubo][lado]# muda, permite mudar a estação de rádio, alterando o lado do Mi Cube.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

O cenário acima, ativado quando o estado de #[Nenhum][Cube][Botão]# muda, permite parar e reiniciar a música ao agitar o Mi Cube.

# Tradução

A interface, as mensagens enviadas nos registos e a documentação estão traduzidas para as 5 línguas suportadas pelo Jeedom (obrigado ao @mips pelo desenvolvimento do ga-translation e do docs-translations). Se forem detetados erros de tradução, pode abrir um pedido de suporte e, se possível, anexar o ficheiro de tradução corrigido (localizado no diretório core/i18n do plugin).

# Opiniões

![MPD_aviso](../images/MPD_avis.png)

Se gostar deste plugin, por favor, deixe uma avaliação e um comentário no Jeedom Market, é sempre um prazer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4464#>

