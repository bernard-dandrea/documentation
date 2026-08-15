
<!--  
Última modificação: 28/07/2026 16:00:31
-->

# Plugin BSBLAN

Plugin que permite a ligação com o controlador BSB-LPB-LAN.

O controlador BSB-LPB-LAN resulta de um projeto cujo objetivo é a comunicação com as placas SIEMENS que controlam inúmeras caldeiras, bombas de calor e outros dispositivos industriais.

A documentação é muito completa e está disponível neste endereço <https://docs.bsb-lan.de>. O equipamento pode ser adquirido junto de Frederik Holst <bsb@code-it.de>.

O BSB-LAN pode substituir vantajosamente os controladores OZW fornecidos pela Siemens. A solução é muito mais económica, permite o acesso a todos os parâmetros das placas da Siemens (ao contrário do OZW) e os tempos de acesso às placas são muito mais rápidos. Além disso, é possível enviar a temperatura das zonas aquecidas sem necessidade de recorrer a um sensor ambiente.

A comunicação entre o plugin e o BSBLAN é feita através de APIs Web.

# Instalação e configuração do controlador BSBLAN

Para que o plugin funcione corretamente, é necessário que o módulo BSB-LAN esteja operacional.

Para a instalação e configuração, consulte a excelente documentação disponível no site do projeto.

Se pretender alterar os parâmetros, terá de autorizar essa ação na configuração do BSBLAN.

O plugin foi testado com as versões 3.2 e 4.2. Em princípio, o plugin deverá funcionar com versões anteriores, uma vez que as chamadas às APIs são bastante básicas e devem existir há já várias versões.

# Configuração do plugin

Depois de instalar o plugin, é necessário ativá-lo.

![Configuração](../images/BSBLAN_configuration.png)

Também pode definir se é utilizada uma tarefa cron autónoma. Isto permite evitar que outras tarefas cron fiquem bloqueadas caso a tarefa cron do plugin fique bloqueada e evita que esta seja bloqueada por outras tarefas cron executadas por outros plugins.

Pode ativar o nível de registo «Debug» para acompanhar a atividade do plugin e identificar eventuais problemas.

# Configuração dos equipamentos

A configuração dos equipamentos está acessível a partir do menu do plugin (menu Plugins, Objetos Conectados e, em seguida, BSBLAN).

Clique em «Adicionar» para configurar o controlador BSBLAN.

![BSBLAN_Equipamento](../images/BSBLAN_Equipement.png)

Indique a configuração do BSBLAN:

-   **Nome**: nome do BSBLAN
-   **Objeto pai**: indica o objeto pai ao qual o equipamento pertence
-   **Categoria**: indica a categoria Jeedom do equipamento
-   **Ativar**: permite ativar o equipamento
-   **Visível**: torna-o visível no painel de controlo
-   **Endereço IP**: IP do equipamento
-   **Conta e palavra-passe**: códigos de acesso ao servidor WEB
-   **Passkey**: prefixo a indicar nas solicitações HTML (ver documentação BSBLAN)
-   **Timeout**: tempo máximo durante o qual se aguarda uma resposta à solicitação HTTP (15 segundos se o campo estiver vazio)
-   **Atualizações**: método utilizado para efetuar as atualizações, seja através de JSON ou de um comando direto na URL. Em alguns casos, verificou-se que as atualizações via JSON não eram efetuadas. Não foi possível compreender o motivo. Nesse caso, pode-se utilizar a opção com o comando /S, que funciona em todas as situações. No entanto, na versão 3 do BSBLAN, alguns comandos que exigem a especificação do sinalizador INFO (por exemplo, enviar a temperatura ambiente) não são considerados.
-   **Número de tentativas**: número de vezes que o comando é enviado em caso de falha (3 se o campo estiver vazio)
-   **Ícone**: permite selecionar um tipo de ícone para o equipamento no painel de configuração

É possível associar um ícone específico ao BSBLAN. Também é possível personalizar um ícone do tipo «perso», adicionando a imagem correspondente (por exemplo, «perso1.png» para o ícone «perso1») no diretório «plugin_info» do plugin.

Os botões seguintes permitem as seguintes funções:

-   **Aceder ao BSBLAN**: permite iniciar uma sessão Web no BSBLAN
-   **Testar a ligação ao BSBLAN**: permite verificar se os parâmetros de ligação estão corretos (não se esqueça de guardar a configuração antes de clicar no botão). É apresentado o número da versão do BSBLAN.

# Comandos associados aos equipamentos

![BSBLAN_Comandos](../images/BSBLAN_Commandes.png)

Por predefinição, são criados dois comandos:

- Última atualização: comando de informação que indica quando a informação mais recente do BSBLAN foi atualizada
- Refresh: comando que permite atualizar todos os parâmetros para os quais a atualização está ativada

Estão disponíveis os seguintes botões:

- Importar um parâmetro: permite criar um comando de informação para um parâmetro específico
- Adicionar um comando «refresh»: permite forçar a atualização do valor do parâmetro
- Adicionar um comando de ação: permite alterar o valor do parâmetro (quando tal for permitido no servidor WEB)

# Análise dos campos do pedido

Para cada comando relacionado com um parâmetro, além dos campos habituais do Jeedom, encontram-se:

- o LogicalID:
  - para comandos do tipo «info», igual ao código do parâmetro
  - para comandos de ação, igual a «A_» seguido do código do parâmetro
  - para os comandos de atualização, igual a «R_» seguido do código do parâmetro
- a opção de atualização que permite solicitar ou não a atualização do parâmetro
- para os comandos info, o campo scan que indica a frequência de atualização do parâmetro
- para os comandos de ação, o campo «MAJ» que permite especificar um modo de atualização específico

# Widget

![BSBLAN_Widget](../images/BSBLAN_Widget.png)

Eis um exemplo de widget. É possível alterar o nome dos comandos para que sejam mais descritivos.

# Opiniões

![BSBLAN_aviso](../images/BSBLAN_avis.png)

Se gostar deste plugin, por favor, deixe uma avaliação e um comentário no Jeedom Market, é sempre um prazer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4424#>
