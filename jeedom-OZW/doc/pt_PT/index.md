<!--  
Última modificação: 28/07/2026 15:35:23
-->


# Plugin OZW

Plugin que permite a ligação com as centrais de comunicação SIEMENS do tipo OZW.

As centrais de comunicação OZW são utilizadas para comunicar com as placas que controlam diversas caldeiras, bombas de calor e outros dispositivos industriais. Estas centrais dispõem de um servidor Web integrado, a partir do qual é possível controlar os dispositivos que estão ligados às mesmas.

Existem dois modelos com um funcionamento praticamente idêntico:

- OZW672 para comunicação com os dispositivos diretamente no barramento LPB, BSB
- OZW772 para comunicação com os dispositivos através do protocolo KNX

A comunicação entre o plugin e o OZW é efetuada através das APIs WEB fornecidas pela SIEMENS, que permitem simular as interações normalmente realizadas no servidor WEB.

Este plugin é uma evolução significativa do plugin OZW672 (ver https://github.com/NextDom/plugin-ozw672), que já não é mantido e não funciona na versão atual do Jeedom.

# Instalação e configuração do controlador OZW

Para a instalação da central de comunicação WEB, consulte a documentação correspondente da SIEMENS.

![OZW_WEB_ACCESS](../images/OZW_WEB_ACCESS.png)

Ativar o acesso às APIs WEB (menu Início > 0.5 OZWx72.01 > Definições > Comunicação > Serviços).

O plugin foi testado com a versão 12 do servidor WEB. Em princípio, o plugin deverá funcionar com versões anteriores, uma vez que as chamadas às APIs são bastante básicas e devem existir há já várias versões.

![OZW_página inicial](../images/OZW_accueil.png)

Após a instalação, deverá aparecer uma página Web semelhante a esta.

Nesta configuração, existem 2 dispositivos:

-   o primeiro representa uma placa LMS14 que controla uma caldeira
-   o segundo representa a central de comunicação OWZ672 e permite a sua configuração

![OZW_dispositivo](../images/OZW_device.png)

Os diferentes pontos de dados definidos para o mapa estão acessíveis. É possível consultá-los e, se necessário, alterá-los.

Nas APIs fornecidas pela SIEMENS, os pontos de dados devem ser especificados através da respetiva referência WEB, que pode ser encontrada na interface WEB.

![OZW_ponto de dados_referência](../images/OZW_datapoint_reference.png)

Para a encontrar, selecione a linha correspondente e inicie a inspeção do elemento (normalmente, clique com o botão direito do rato e selecione «Inspecionar»). No código correspondente, encontrará um número na instrução «openDialog('xxx')» ou «id='dpxxx'» que indica a referência WEB, 591 no exemplo acima.

![OZW_ID_menu](../images/OZW_ID_menu.png)

Da mesma forma, o ID de um menu pode ser necessário e é encontrado da mesma forma, 590 no exemplo acima.

# Configuração do plugin

Depois de instalar o plugin, é necessário ativá-lo.

![Configuração](../images/OZW_configuration.png)

Também pode definir se é utilizada uma tarefa cron autónoma. Isto permite evitar que outras tarefas cron fiquem bloqueadas caso a tarefa cron do plugin fique bloqueada e evita que esta seja bloqueada por outras tarefas cron executadas por outros plugins.

Pode ativar o nível de registo «Debug» para acompanhar a atividade do plugin e identificar eventuais problemas.

# Configuração dos equipamentos

A configuração dos equipamentos está acessível a partir do menu do plugin (menu Plugins, Objetos Conectados e, em seguida, OZW).

Clique em «Adicionar» para definir o OZW.

![OZW_Equipamento_OZW](../images/OZW_Equipement_OZW.png)

Indique a configuração do OZW:

-   **Nome**: nome do OZW
-   **Objeto pai**: indica o objeto pai ao qual o equipamento pertence
-   **Categoria**: indica a categoria Jeedom do equipamento
-   **Ativar**: permite ativar o equipamento
-   **Visível**: torna-o visível no painel de controlo
-   **Endereço IP**: IP do equipamento
-   **Conta e palavra-passe**: códigos de acesso ao servidor WEB
-   **Duração de uma sessão**: período após o qual o ID da sessão é renovado
-   **Ícone**: permite selecionar um tipo de ícone para o equipamento no painel de configuração

Depois de guardar o OZW, os seguintes botões ficam ativos:

-   **Aceder ao OZW**: permite iniciar uma sessão Web no OZW
-   **Importar dispositivos**: permite importar os equipamentos correspondentes aos dispositivos ligados ao OZW.

![OZW_Equipamento_OZW_dispositivos](../images/OZW_Equipement_OZW_devices.png)

No exemplo acima, após a importação dos dispositivos, encontramos:

- o OZW672 como equipamento principal
- o OZW672.01 como dispositivo
- a placa LMS14 que controla a caldeira

![OZW_Equipamento_OZW_dispositivo](../images/OZW_Equipement_OZW_device.png)

É possível associar um ícone específico ao dispositivo. Também é possível personalizar um ícone do tipo «perso» adicionando a imagem correspondente (por exemplo, «perso1.png» para o ícone «perso1») no diretório «plugin_info» do plugin.

# Comandos associados aos equipamentos

![OZW_Comandos](../images/OZW_Commandes.png)

Para o OZW, são criados dois comandos do tipo «info»:

- Estado: igual a 1 quando a comunicação com o servidor WEB está estabelecida; 0 caso contrário
- SessionID: ID utilizada pelas APIs Web

![OZW_Comandos_device_initial](../images/OZW_Commandes_device_initial.png)

Para os dispositivos ligados ao OZW, são criados dois comandos:

- Última atualização: comando do tipo «info» que indica quando a informação mais recente do dispositivo foi atualizada
- Refresh: comando do tipo «ação» que permite atualizar todos os pontos de dados para os quais a atualização está ativada

![OZW_Importer_Menu_principal](../images/OZW_Importer_Menu_principal.png)

O botão «Importar comandos principais» no separador «Equipamento» permite importar todos os pontos de dados do menu denominado «móvel». Este menu está disponível na aplicação Android fornecida pela SIEMENS e não está disponível para todos os dispositivos. A criação dos comandos pode demorar vários minutos. Após a execução, os principais pontos de dados do dispositivo são definidos como comandos do tipo «info».

![OZW_import_menu_específico](../images/OZW_import_menu_specifique.png)

Da mesma forma, o botão «Importar menu» no separador «Equipamento» permite importar todos os pontos de dados de um menu específico. Para tal, é necessário indicar a referência WEB do menu.


![OZW_botões_importar_encomenda](../images/OZW_boutons_import_commande.png)

No separador «Comandos», estão disponíveis os seguintes botões:

- Importar um ponto de dados: permite criar um comando de informação para um ponto de dados específico
- Adicionar uma ação: permite alterar o valor do ponto de dados (quando permitido no servidor WEB)
- Adicionar um comando de atualização: permite forçar a recuperação do valor do ponto de dados

**Atenção**: indique a referência WEB do datapoint e não o número de linha apresentado na linha do datapoint.

# Análise dos campos do pedido

![OWZ_Análise_de_comandos](../images/OWZ_Analyse_commande.png)

Para cada comando relacionado com um ponto de dados, além dos campos habituais do Jeedom, encontram-se:

- o LogicalID:
  - para comandos do tipo «info», igual à referência WEB do ponto de dados
  - para comandos de ação, igual a «A_» seguido da referência WEB do ponto de dados
  - para os comandos de atualização, igual a «R_» seguido da referência WEB do ponto de dados
- a opção de atualização que permite solicitar ou não a atualização do ponto de dados
- o campo «scan», que indica a frequência de atualização do ponto de dados

# Widget

![OZW_widget](../images/OZW_widget.png)

Eis um exemplo de widget. É possível alterar o nome dos comandos para refletir o número da linha indicado no servidor Web.

# Tradução

A interface, as mensagens enviadas nos registos e a documentação estão traduzidas para as 5 línguas suportadas pelo Jeedom (obrigado ao @mips pelo desenvolvimento do ga-translation e do docs-translations). Se forem detetados erros de tradução, pode abrir um pedido de suporte e, se possível, anexar o ficheiro de tradução corrigido (localizado no diretório core/i18n do plugin).

# Opiniões

![OZW_aviso](../images/OZW_avis.png)

Se gostar deste plugin, por favor, deixe uma avaliação e um comentário no Jeedom Market, é sempre um prazer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4414#>
