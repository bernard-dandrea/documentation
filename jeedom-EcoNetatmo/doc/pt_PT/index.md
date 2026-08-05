
<!--  
Última modificação: 26/07/2026 18:45:10
-->


# Plugin EcoNetatmo

Plugin que permite recolher os dados de consumo dos contadores ecológicos Legrand do tipo Drivia com NetAtmo (ref. 41203x).

Este plugin foi desenvolvido com base no plugin padrão netatmoWeather.

Este plugin utiliza as APIs fornecidas pela Netatmo (ver o link seguinte <https://dev.netatmo.com/apidocumentation/control>).

# Recuperação das informações de início de sessão

Para aceder aos dados do seu Ecocompteur, tem de dispor de um client\_id e de um client\_secret gerados no site <https://dev.netatmo.com>.

Se ainda não o fez, crie uma conta <https://auth.netatmo.com/fr-fr/access/signup?next_url=https%3A%2F%2Fdev.netatmo.com%2Fbusiness-showcase>

![aplicações](../images/apps.png)

Depois de iniciar sessão, aceda ao menu de aplicações ( <https://dev.netatmo.com/apps/> ) e clique em «Create».

![aplicação](../images/app.png)

Preencha o formulário e clique em «Guardar».

![segredo](../images/secret.png)

O «ID do cliente» e o «segredo do cliente» foram gerados. Pode utilizá-los para configurar o plugin.


# Recuperação de tokens

Os tokens permitem o acesso aos seus dados nos servidores da Netatmo (consulte a norma de autorização OAuth 2).

É possível gerá-los diretamente na página da aplicação.

![gerar_token](../images/generate_token.png)

Selecione o âmbito «read_magellan» e clique em «Gerar token».

![tokens](../images/tokens.png)

Depois de autorizar o acesso aos seus dados, são gerados tokens.

# Configuração do plugin

Depois de instalar o plugin, é necessário ativá-lo e introduzir os seus dados de início de sessão da Netatmo:

![configuração](../images/configuration.png)

-   **ID do cliente**: o seu ID de cliente (ver secção de configuração)
-   **Cliente secreto**: o seu cliente secreto (ver secção de configuração)
-   **Token de acesso**: token que permite o acesso aos seus dados nos servidores da Netatmo
-   **Token de atualização**: token que permite atualizar o token de acesso

A gestão dos tokens é efetuada pelo plugin. Caso estes se tornem inválidos (ver os registos), por exemplo, após um longo período de inatividade, será necessário gerar novos tokens e atualizar a configuração do plugin com os novos tokens.

Também pode definir se é utilizada uma tarefa cron autónoma. Isto permite evitar que outras tarefas cron fiquem bloqueadas caso a tarefa cron do plugin fique bloqueada e evita que esta seja bloqueada por outras tarefas cron executadas por outros plugins.

![registo](../images/log.png)

Pode ativar o nível de registo «Debug» para acompanhar a atividade do plugin e identificar eventuais problemas.

# Configuração dos equipamentos

A configuração dos equipamentos Netatmo está disponível a partir do menu do plugin (menu Plugins, Energia e, em seguida, EcoNetAtmo):

![sincronização](../images/synchronisation.png)

Clique em «Sincronização» para iniciar a criação dos dispositivos. A API /homesdata é utilizada para recuperar as informações (ver <https://dev.netatmo.com/apidocumentation/control#homesdata>).

![equipamentos](../images/equipements.png)

Os contadores das linhas elétricas foram criados. Existe um equipamento por linha.

![equipamento](../images/equipement.png)

Na secção «Equipamento», encontrará toda a configuração do seu equipamento:

-   **Nome**: nome do seu contador (este é retirado da configuração do Netatmo)
-   **Objeto pai**: indica o objeto pai ao qual o equipamento pertence
-   **Categoria**: indica a categoria Jeedom do equipamento
-   **Ativar**: permite ativar o seu equipamento
-   **Visível**: torna-o visível no painel de controlo
-   **ID do módulo**: indica o identificador único do equipamento na Netatmo
-   **Tipo de consumo**: indica o tipo do seu equipamento na Netatmo
-   **Tipo de fonte**: indica a fonte de energia do seu equipamento na Netatmo
-   **Ícone**: permite selecionar um tipo de ícone para o seu equipamento no painel de configuração
  
O botão «Importar contadores» permite criar os comandos correspondentes ao equipamento. Isto é feito durante a criação do equipamento e só é útil se tiver eliminado um comando.

![comandos](../images/comandos.png)

No separador «Comandos» encontra a lista de comandos (estes são gerados aquando da criação do equipamento).

O comando «Refresh» permite iniciar a recolha imediata dos valores dos contadores. Por predefinição, é iniciada uma recolha a cada 10 minutos.

Os restantes comandos correspondem aos contadores alimentados pela Netatmo (ver a API /getmesure <https://dev.netatmo.com/apidocumentation/control#getmeasure>). Para cada um deles, além dos valores habituais do Jeedom, encontram-se:

-   o nome apresentado no painel de controlo
-   o logicalID que corresponde ao «tipo» na API da Netatmo
-   a possibilidade de ativar ou não a leitura do contador
-   o período que corresponde ao «scale» na API da Netatmo (para o qual se pretende recuperar os dados; apenas são apresentados os valores autorizados pela API da Netatmo)

# Widget

![widget](../images/widget.png)

Este é o widget padrão.

# Perguntas frequentes

>**Qual é a frequência de atualização?**
>
>O plugin recolhe as informações a cada 10 minutos. No entanto, o contador de energia envia as suas leituras aproximadamente a cada 3 horas, pelo que é possível observar este desfasamento na recolha dos dados.

>**Posso recolher os contadores de gás e de água?**
>
>O plugin é capaz de fazer isso. Infelizmente, a API da Netatmo não especifica qual é o «tipo» a utilizar para a recuperação destes valores. Foi enviada uma solicitação à equipa responsável pelo desenvolvimento da API, mas ainda não foi dada qualquer resposta.

# Tradução

A interface, as mensagens enviadas nos registos e a documentação estão traduzidas para as 5 línguas suportadas pelo Jeedom (obrigado ao @mips pelo desenvolvimento do ga-translation e do docs-translations). Se forem detetados erros de tradução, pode abrir um pedido de suporte e, se possível, anexar o ficheiro de tradução corrigido (localizado no diretório core/i18n do plugin).

# Opiniões

![EcoNetatmo_opinião](../images/EcoNetatmo_avis.png)

Se gostar deste plugin, por favor, deixe uma avaliação e um comentário no Jeedom Market, é sempre um prazer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4413#>
