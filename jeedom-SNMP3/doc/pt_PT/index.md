
<!--  
Última modificação: 25/07/2026 18:39:50
-->

# Plugin SNMP3

Plugin que permite ler e escrever os OIDs dos dispositivos compatíveis com o protocolo SNMP.

O SNMP é um dos protocolos amplamente aceites para gerir e analisar os elementos da rede. A maioria dos elementos de rede de nível profissional vem equipada com um agente SNMP integrado.

O plugin utiliza o pacote php-snmp (ver <https://www.php.net/manual/fr/book.snmp.php>), que é um wrapper da biblioteca Net-SNMP (ver <http://www.net-snmp.org>). O plugin permite consultar (comando get) e atualizar (comando set) os OIDs que suportam esta funcionalidade.

# AVISO

Este plugin destina-se a pessoas que estão familiarizadas com o protocolo.

Este não é particularmente complicado, mas requer, ainda assim, o domínio dos conceitos subjacentes (autenticação, OID, MIB, ...).

Antes de contactar o programador em caso de eventuais problemas, verifique primeiro se as configurações para a comunicação com o dispositivo SNMP estão corretas.

Para tal, pode-se utilizar, numa sessão SSH, o comando snmpget, por exemplo:

snmpget -v 3 -n "" -u admin_snmp_2024 -a MD5 -A "xxxxxx" -x DES -X "yyyyy" -l authPriv 192.168.1.5 .1.3.6.1.2.1.1.6.0

![SNMP3_snmp_get](../images/SNMP3_snmp_get.png)

# Instalação e configuração de dispositivos SNMP

Para que o plugin funcione corretamente, é necessário que o protocolo SNMP esteja devidamente instalado e configurado no sistema de destino. Consulte a documentação do fabricante para efetuar esta configuração.

Recomenda-se a utilização do protocolo v3 para garantir a segurança da ligação.

![SNMP3_Synology](../images/SNMP3_Synology.png)

Veja acima um exemplo de configuração num NAS Synology.

Teste os parâmetros de ligação com o comando snmpget (ver parágrafo anterior) ou outros utilitários.

# Configuração do plugin

Depois de instalar o plugin, é necessário ativá-lo. O pacote php-snmp é instalado durante a instalação das dependências.

Pode ativar o nível de registo «Debug» para acompanhar a atividade do plugin e identificar eventuais problemas.


![SNMP3_Equipamento](../images/SNMP3_cron.png)

Também pode definir se é utilizada uma tarefa cron autónoma. Isto permite evitar que outras tarefas cron fiquem bloqueadas caso a tarefa cron do plugin fique bloqueada e evita que esta seja bloqueada por outras tarefas cron executadas por outros plugins.

# Gestão de MIBs

Os OIDs podem ser identificados pelo seu código numérico, por exemplo, .1.3.6.1.4.1.6574.1.1.0, ou utilizando a MIB correspondente, por exemplo, SYNOLOGY-SYSTEM-MIB::systemStatus.0.

Ao instalar o pacote php-snmp, são instalados vários MIBs (normalmente no diretório /usr/share/snmp/mibs), que podem ser utilizados diretamente.

O plugin permite instalar MIBs específicas, colocando os ficheiros correspondentes, por exemplo, SYNOLOGY-SYSTEM-MIB.txt, no diretório plugins/SNMP3/data/mibs.

Também pode copiar os ficheiros para o diretório comum (normalmente /usr/share/snmp/mibs). Tenha em atenção que será necessário repetir este procedimento em caso de restauração do Jeedom.

Se tiver dificuldades na implementação dos MIBs, pode testá-los com o comando snmptranslate (consulte <https://net-snmp.sourceforge.io/tutorial/tutorial-5/commands/snmptranslate.html>). Atenção: neste caso, os MIBs no diretório plugins/SNMP3/data/mibs não são tidos em conta.

# Configuração dos equipamentos

A configuração dos equipamentos está acessível a partir do menu do plugin (menu Plugins, Objetos Conectados e, em seguida, SNMP3).

Clique em «Adicionar» para configurar o dispositivo SNMP.

![SNMP3_Equipamento](../images/SNMP3_Equipement.png)

Indique a configuração do dispositivo SNMP:

-   **Nome**: nome do dispositivo SNMP
-   **Objeto pai**: indica o objeto pai ao qual o equipamento pertence
-   **Categoria**: indica a categoria Jeedom do equipamento
-   **Ativar**: permite ativar o equipamento
-   **Versão**: versão do SNMP
-   **localhost**: IP do equipamento
-   **Parâmetros de segurança**: consulte <https://www.php.net/manual/fr/snmp.setsecurity.php>
-   **timeout**: tempo máximo durante o qual se aguarda uma resposta à solicitação SNMP
-   **retries**: número de vezes que o comando é enviado em caso de falha (3 se o campo estiver vazio)
-   **Ícone**: permite selecionar um tipo de ícone para o equipamento no painel de configuração

É possível personalizar um ícone específico adicionando a imagem correspondente (por exemplo, perso1.png para o ícone perso1) no diretório plugin_info do plugin.

O botão **Testar a ligação ao SNMP3** permite verificar se os parâmetros de ligação estão corretos (não se esqueça de ligar o equipamento e guardar a configuração antes de clicar no botão).

# Comandos associados aos equipamentos

![SNMP3_Comandos](../images/SNMP3_Commandes.png)

Por predefinição, são criados dois comandos:

- Última atualização: comando de informação que indica quando a informação mais recente do dispositivo SNMP foi atualizada
- Refresh: comando que permite atualizar todos os OIDs para os quais a atualização está ativada

Estão disponíveis os seguintes botões:

- Importar um OID: permite criar um comando de informação para um OID
- Adicionar um comando «refresh»: permite criar um comando de ação para forçar a atualização do valor do OID
- Adicionar uma ação: permite criar um comando de ação para alterar o valor do OID (quando permitido pelo dispositivo SNMP)

# Análise dos campos do pedido

Para cada comando relacionado com um OID, além dos campos habituais do Jeedom, encontram-se:

- o LogicalID:
  - para comandos do tipo «info», iguais ao OID
  - para os comandos de atualização, igual a «R_» seguido do OID
  - para comandos de ação, igual a «A_» seguido do OID
- a opção de atualização que permite solicitar ou não a atualização do OID
- o campo «scan», que indica a frequência de atualização do OID

Para os comandos que permitem a atualização do OID, o subtipo do comando de ação determina o formato do valor transmitido ao dispositivo SNMP. Quando o subtipo é «Message», o título indica o formato e o conteúdo da mensagem indica o valor (apenas a primeira linha é transmitida). Consulte <https://www.php.net/manual/fr/function.snmpset.php> para ver os formatos suportados.

# Widget

![SNMP3_Widget](../images/SNMP3_Widget.png)

Eis um exemplo de widget. É possível alterar o nome dos comandos para que sejam mais descritivos.

# Opiniões

![SNMP3_aviso](../images/SNMP3_avis.png)

Se gostar deste plugin, por favor, deixe uma avaliação e um comentário no Jeedom Market, é sempre um prazer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4484#>
