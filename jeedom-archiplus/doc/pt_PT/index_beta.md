<!--  
Última modificação: 26/06/2026
-->
- [A gestão dos registos históricos no Jeedom](#a-gestão-dos-registos-históricos-no-jeedom)
  - [Funcionamento](#funcionamento)
  - [Volume dos registos históricos](#volume-dos-registos-históricos)
  - [Os limites do arquivo no Jeedom](#os-limites-do-arquivo-no-jeedom)
  - [As VANTAGENS do plugin archiplus](#as-vantagens-do-plugin-archiplus)
  - [Aviso](#aviso)
- [Plugin archiplus](#plugin-archiplus)
  - [Instalar o plugin archiplus](#instalar-o-plugin-archiplus)
  - [Configurar o plugin](#configurar-o-plugin)
  - [Os módulos do plugin](#os-módulos-do-plugin)
- [Acesso aos módulos](#acesso-aos-módulos)
  - [Os botões de comando](#os-botões-de-comando)
  - [A coluna de seleção de linhas](#a-coluna-de-seleção-de-linhas)
  - [Os cabeçalhos das colunas](#os-cabeçalhos-das-colunas)
  - [As linhas](#as-linhas)
  - [Os totais no final da tabela](#os-totais-no-final-da-tabela)
- [o módulo Monitor](#o-módulo-monitor)
  - [Estatísticas](#estatísticas)
  - [Visualização](#visualização)
  - [Alterações](#alterações)
  - [Alterações a partir de um ficheiro Excel](#alterações-a-partir-de-um-ficheiro-excel)
  - [Dados editáveis](#dados-editáveis)
    - [KLV (Manter o último valor)](#klv-keep-last-value)
    - [Uniq](#uniq)
    - [Prazo](#prazo)
    - [Enquadramento](#enquadramento)
    - [Pond](#pond)
    - [Pacote](#pack)
    - [Arredondamento](#arredondamento)
  - [Funções acessíveis através do menu contextual](#funções-acessíveis-através-do-menu-contextual)
- [Dados históricos](#dados-históricos)
  - [Acesso](#acesso)
  - [Alteração](#alteração)
  - [Eliminação](#eliminação)
  - [Exportar](#export)
- [O módulo Import](#o-módulo-import)
- [O módulo Restore](#o-módulo-restore)
- [Perguntas frequentes](#faq)
  - [Manter o último valor](#keep-last-value)
  - [Uniq](#uniq-1)
  - [Prazo e âmbito](#prazo-e-âmbito)
  - [Suavização e ponderação](#suavização-e-ponderação)
  - [Pacote](#pack-1)
  - [Arredondado](#arrondado-1)
  - [Copiar os dados do historyArch para o history](#copiar-os-dados-do-historyarch-para-o-history)
  - [Utilizar o Archiplus em PHP](#utilizar-archiplus-em-php)
- [Os registos](#os-registos)
- [Tradução](#tradução)
- [Opiniões](#opiniões)



A principal função do plugin é fornecer um conjunto completo de ferramentas que permitem:

*   **gerir as definições de arquivo das encomendas do tipo INFO**
*   **visualizar os volumes de dados e detetar anomalias**
*   **inserir facilmente dados históricos a partir de ficheiros do tipo Excel**
*   **recuperar os registos históricos a partir dos arquivos do Jeedom**
*   **alargar as opções de arquivo padrão do Jeedom**

A ativação opcional do arquivo integrado no plugin permite alargar significativamente as funcionalidades de arquivo oferecidas pelo Jeedom.

# Gestão do histórico no Jeedom

## Funcionamento

O histórico no Jeedom pouco evoluiu desde as primeiras versões e baseia-se em duas tabelas:

* a tabela «history», que recebe as atualizações dos valores dos comandos do tipo INFO para os quais o histórico está ativado
* a tabela historyArch, que recebe, em cada arquivamento (normalmente todos os dias às 5h00), os valores históricos, consolidados ou não, consoante a configuração definida para o comando.

A estrutura das duas tabelas é idêntica e muito simples: cada comando é registado com um valor de Id e datetime (gerido ao segundo).

O histórico pode ser apresentado na interface do Jeedom sob a forma de um gráfico.

A documentação oficial relativa à gestão dos registos no Jeedom encontra-se [aqui](https://doc.jeedom.com/fr_FR/core/4.5/history).

## Volume dos registos históricos

O utilizador do Jeedom começará a interessar-se pelo histórico quando verificar que a base de dados está a crescer de forma exagerada, que o tempo de visualização do histórico está a tornar-se muito longo e que o tamanho das cópias de segurança não pára de aumentar.

A ligação seguinte permite aceder a um tutorial que explica como criar um cenário que irá listar os volumes das tabelas mais volumosas e os comandos INFO com os históricos mais extensos [Tutorial - Analisar os arquivos](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

De forma mais simples, pode ver os volumes das tabelas consultando diretamente a base de dados (menu Definições / Sistema / Configuração, depois o separador SO / BD (o último), depois o botão «Administração da base de dados» (botão vermelho mais abaixo) e, à esquerda, a consulta «tamanho»).

Numa instalação padrão, é necessário começar a analisar a situação quando o volume total dos arquivos ultrapassar um milhão de registos ou quando um comando «info» apresentar mais de 10 000 registos. Nesse caso, é necessário analisar os comandos em questão e ajustar os diferentes parâmetros de registo e arquivo, de modo a reduzir esse volume. Se tal não for possível, poderá ser necessário recorrer a outros métodos de arquivo, por exemplo, o InfluxDB, que pode ser integrado de forma padrão com o Jeedom.

O plugin archiplus apresenta imediatamente os volumes do history e do historyArch e permite identificar facilmente os problemas e encontrar soluções para os mesmos.

## Os limites do arquivo no Jeedom

Embora, em muitas instalações, o funcionamento padrão seja suficiente, é possível identificar as seguintes limitações:

* Dificuldade em visualizar e alterar os parâmetros de arquivo: a única ferramenta disponível (menu Análise / Histórico e, em seguida, Configuração) é muito lenta, pouco prática e apresenta poucos campos para configurar
* Dificuldade em visualizar os volumes históricos por pedido e identificar volumes anormais: é necessário recorrer a consultas SQL e a processos pouco práticos
* Parâmetros para o agrupamento de dados no historyArch definidos globalmente e não personalizáveis por comando
* Falta de visibilidade relativamente ao processo de arquivo (sem registo)
* Arquivamento global: não é possível iniciar o arquivamento para uma encomenda específica
* Suavização por média aproximada
* Ferramentas básicas para exportar/importar dados (plug-in dataexport). Não existe nenhuma opção para restaurar os dados históricos contidos nas cópias de segurança.

## As VANTAGENS do plugin archiplus

O plugin archiplus permite visualizar numa tabela os comandos do tipo INFO com todos os parâmetros relativos ao arquivo. O número de registos em history e historyArch também é apresentado, o que permite detetar muito facilmente volumes excessivos. O plugin utiliza a biblioteca JavaScript Tabulator, que é extremamente eficiente e permite um acesso muito fácil às funções do plugin.

Todas as funcionalidades oferecidas pelo Jeedom estão disponíveis diretamente e foram adicionadas outras funcionalidades:

* Configuração avançada do comando
* Visualização de gráficos e extração de dados
* Limpar o histórico
* Exportação CSV padrão
* Copiar a configuração do histórico (ou de um único parâmetro) para vários comandos
* Carregamento das configurações dos comandos INFO relativos ao histórico a partir de um ficheiro Excel
* Início do arquivamento para um pedido específico
* Copiar o histórico de uma encomenda para outra encomenda
* Cópia do «historyArch» para o «history», a fim de iniciar uma consolidação por intervalo
* Importação do histórico de uma encomenda a partir de um ficheiro Excel
* Extracção do histórico em vários formatos (xlsx, CSV, JSON, HTML) de um ou mais comandos a partir do Jeedom ou de uma cópia de segurança padrão do Jeedom
* Extracção, a partir de uma cópia de segurança do Jeedom, dos parâmetros dos comandos INFO relativos ao histórico (estes parâmetros podem ser posteriormente aplicados no Jeedom)

Além disso, o processo de arquivamento do plugin pode ser ativado em substituição da função de arquivamento nativa oferecida pelo Jeedom. Este permite:

* iniciar o arquivamento para um pedido específico
* registar no registo Archiplus o conjunto das operações realizadas e os parâmetros tidos em conta para cada comando
* personalizar o período de cálculo (para mínimo, máximo e média), o prazo antes do arquivo e o tamanho do pacote para cada comando
* definir a data de limpeza para um dia, uma hora ou um minuto
* iniciar o arquivamento de um pedido a partir de um cenário (em código PHP)
* adicionar opções não previstas no Jeedom (ver as explicações mais adiante na documentação)
  * Keep Last Value: manter sempre pelo menos um valor no histórico
  * Uniq: eliminar valores consecutivos idênticos no historyArch
  * Ponderação: no alisamento por média, calcular o valor ponderado ao longo do intervalo (e não a média dos valores)

O plugin archiplus foi desenvolvido no Debian 12 e não utiliza o jQuery (tal como as bibliotecas de terceiros utilizadas). Cumpre as normas de desenvolvimento do Jeedom. O código da classe archiplus é muito bem estruturado e amplamente documentado: o autor do plugin analisará todas as propostas de correção ou melhoria.

Uma vez que o Jeedom não tem um plano de evolução para a gestão do histórico, o plugin não deverá necessitar de uma reformulação num futuro próximo.

## Aviso

O plugin e o seu processo específico de arquivo foram testados de forma muito rigorosa, mas não estão, no entanto, isentos de anomalias. Nesse caso, a equipa do Jeedom não é, obviamente, obrigada a prestar assistência. Os pedidos de análise e correção devem ser dirigidos obrigatoriamente ao autor do plugin através do pedido de assistência padrão.

A ativação do plugin e, em particular, do processo de arquivamento, implica, portanto, a aceitação total desta situação.

# Plugin Archiplus

## Instalar o plugin archiplus

Aceda ao Jeedom Market, procure o plugin archiplus e instale a versão **estável**. Em seguida, **ative o plugin**.

![001](../images/001.png)

O plugin está acessível através do menu.

## Configurar o plugin

Na configuração, pode definir os parâmetros habituais dos plugins e os valores predefinidos do plugin.

![003](../images/003.png)

Para obter o máximo de informações sobre o processo de arquivamento do plugin e as ações realizadas, recomenda-se colocar os registos no modo Debug.

Note-se que os pedidos de assistência devem ser efetuados através do botão **Assistência**.

![002](../images/002.png)

Na secção de configuração, pode:

* Ativar o arquivo específico (desativado por predefinição)
* Indique se os registos em history e historyArch devem ser eliminados caso o comando em questão não exista
* Optar por não transferir os registos do histórico para o historyArch quando não houver suavização
* Definir o formato para as exportações
* Definir o intervalo predefinido para as datas de eliminação e de fim de arquivo

A ativação do arquivamento específico cria um novo cron no motor de tarefas e desativa o arquivamento padrão. A desativação do arquivamento específico realiza a operação inversa.

Se quiser testar o processo de arquivamento do plugin, pode ativá-lo temporariamente, realizar testes de arquivamento em comandos individuais e, em seguida, desativar o arquivamento do plugin. Como o processo de arquivamento do Jeedom é normalmente iniciado às 5 da manhã, não haverá impacto nos comandos não testados.

## Os módulos do plugin

![004](../images/004.png)

A partir do menu Plugins / Monitorização / archiplus, tem acesso a todas as funcionalidades do plugin

* Configuração do plugin (ver acima)
* Acesso às definições globais da configuração do arquivo
* Monitorização: visualizar e alterar as configurações e realizar as principais operações relacionadas com o arquivo
* Importação: importar dados históricos a partir de um ficheiro do tipo Excel
* Restauração: extrair dados históricos de um arquivo padrão do Jeedom

A visualização dos dados históricos está disponível a partir do módulo «Monitorização e Recuperação».

# Acesso aos módulos

Os módulos são iniciados a partir da configuração do plugin.

![005](../images/005.png)

A base da interface é uma tabela Tabulator preenchida com os dados relevantes.

Por exemplo, com o módulo Monitor, é apresentado um painel com os comandos do tipo INFO que têm a função de histórico ativada.

O ecrã é composto por várias partes.

## Os botões de comando

![006](../images/006.png)

Os botões permitem realizar ações gerais relacionadas com a visualização, as linhas selecionadas, as atualizações, etc.

![013](../images/013.png)

Os botões acima são comuns a todos os módulos e permitem:

* exibir o ficheiro de registo do Archiplus
* ir para a primeira ou última linha da tabela
* desativar os filtros que foram ativados
* voltar à classificação inicial
* exportar os dados apresentados na tabela (apenas os dados filtrados)
* voltar aos diferentes módulos propostos pela Archiplus

![019](../images/019.png)

O botão padrão «Ajuda na página atual» permite aceder à documentação do plugin.

## A coluna de seleção de linhas

![007](../images/007.png)

A primeira coluna permite selecionar as linhas sobre as quais se pretende atuar.

Ao clicar nos cabeçalhos das colunas, selecionam-se todas as linhas exibidas na tabela.

É possível selecionar cada linha individualmente clicando na caixa de seleção ou em qualquer ponto da linha.

Também é possível selecionar um conjunto de linhas clicando na primeira linha a selecionar, mantendo a tecla Control premida, e, em seguida, clicando na última linha, mantendo a tecla Control premida (tenha o cuidado de clicar em qualquer ponto da linha, mas não na caixa de seleção; caso contrário, a seleção múltipla não funcionará).

## Os títulos das colunas

![008](../images/008.png)

Os títulos das colunas descrevem o conteúdo das células que se encontram nessa coluna.

Permitem:

* obter informações adicionais através de uma caixa de informação, mantendo o cursor do rato sobre o campo durante um segundo
* para ordenar as linhas de acordo com o valor do campo, clicando no título da coluna (note que o botão «Ordenação inicial» permite anular todas as ordenações efetuadas)
* filtrar as linhas apresentadas, introduzindo um critério de seleção no campo situado por baixo do nome da coluna (note-se que o botão «Reset» permite anular todas as seleções).

No caso do módulo Monitor, o agrupamento das colunas permite selecionar apenas determinados tipos de informação.

## As linhas

![009](../images/009.png)

As linhas apresentam as informações solicitadas.

Dependendo do contexto, um clique com o botão direito do rato faz aparecer um menu contextual com as ações possíveis.

![010](../images/010.png)

Ao clicar num campo editável, é possível introduzir um novo valor.

![011](../images/011.png)

Os campos alterados aparecem num fundo magenta, que desaparece após a validação das alterações.

## Os totais na parte inferior da tabela

![012](../images/012.png)

Na parte inferior da tabela são apresentados os totais correspondentes às linhas exibidas ou selecionadas.

# o módulo Monitor

Este é o módulo principal do archiplus.

![005](../images/005.png)

Depois de clicar em «Monitor», os comandos «INFO» com um histórico ativo são apresentados em poucos segundos.

![014](../images/014.png)

Ao clicar no botão acima, é possível alternar para a visualização de todos os comandos INFO, mesmo aqueles que não requerem histórico ou aqueles cujo equipamento está inativo.

## Estatísticas

![016](../images/016.png)

O número de registos em «history» e «historyArch» corresponde geralmente ao da última arquivação (é possível ver a data de atualização ao passar o rato sobre um dos contadores). Ao clicar no cabeçalho da coluna «#All», é possível ver imediatamente os comandos com o maior histórico.

![015](../images/015.png)

Ao clicar no botão acima, é possível reiniciar um cálculo, o que demorará alguns segundos.

![017](../images/017.png)

Os totais na parte inferior da tabela permitem-lhe saber imediatamente o tamanho do seu histórico.

## Visualização

![018](../images/018.png)

Os botões de visualização permitem selecionar os dados apresentados

* configuração do histórico
* os cálculos
* valores proibidos
* visualização através de gráficos
* as estatísticas

Dependendo do que lhe interessa, pode ativar ou não a secção que pretende gerir. Para não sobrecarregar o ecrã inicial do Monitor, apenas são apresentados os dados de identificação, de configuração e as estatísticas.

## Alterações

![020](../images/020.png)

Para alterar um dado, basta clicar na área em questão e introduzir um novo valor.

![021](../images/021.png)

Os dados alterados aparecem com fundo magenta.

![022](../images/022.png)

Ao clicar com o botão direito do rato numa linha, é possível copiar a sua configuração ou um dos seus parâmetros para as linhas selecionadas.

![023](../images/023.png)

Para verificar os dados antes da validação, é possível apresentar apenas as linhas que foram alteradas.

![024](../images/024.png)

Depois de clicar no botão «Validar», os dados são atualizados e o fundo das células alteradas é apagado.

![025](../images/025.png)

Note-se que, ao clicar com o botão direito do rato numa linha, é possível aceder diretamente à configuração avançada de controlo do Jeedom.

## Alterações a partir de um ficheiro Excel

![070](../images/070.png)

Também é possível carregar alterações a partir de um ficheiro Excel ou CSV clicando no botão «Importar». Este botão permite selecionar o ficheiro e carregar os dados alterados na tabela.

![071](../images/071.png)

Os dados devem ter o mesmo formato que o gerado pela exportação. Assim, é possível exportar os dados, alterá-los no Excel e, em seguida, carregar as alterações na tabela.

Também é possível extrair as configurações de arquivo a partir de uma cópia de segurança do Jeedom e carregar as alterações: isto permite ver rapidamente as alterações efetuadas desde a cópia de segurança e, se necessário, voltar a um estado anterior.

![072](../images/072.png)

Após a importação, é possível visualizar apenas os dados alterados clicando no filtro «Atualizações». Também é possível clicar nos botões de visualização (Configuração, Cálculos, ...) para ver todos os dados editáveis.

Para aplicar as alterações, clique no botão «Validar».

## Dados editáveis

Todos os dados de configuração do histórico padrão do Jeedom e os específicos do plugin archiplus podem ser alterados diretamente a partir do Monitor.

A seguir, são detalhadas as opções específicas do Archiplus:

### KLV (Manter o último valor)

Permite manter sempre pelo menos um registo no histórico. Consulte a seguinte FAQ para compreender a utilização desta opção [Keep Last Value](#keep-last-value).

### Uniq

Permite eliminar valores consecutivos idênticos no historyArch (e, eventualmente, no history). Consulte a seguinte FAQ para compreender a utilização desta opção [Uniq](#uniq-1).

### Prazo

Trata-se do intervalo de tempo a partir do qual os registos do histórico são transferidos para o historyArch. Por predefinição no Jeedom, este parâmetro é o mesmo para todos os comandos. Com o archiplus, este intervalo pode ser especificado por comando.

### Enquadramento

Permite definir até que momento os dados históricos são eliminados, bem como o momento da transferência dos dados do history para o historyArch, com base num limite de dia, hora ou minuto. Consulte a seguinte FAQ para compreender a utilização desta opção [Prazo e Intervalo](#prazo-e-intervalo).

### Lago

Permite calcular uma média ponderada tendo em conta o tempo, em vez de uma média dos valores registados ao longo do período. Consulte a seguinte FAQ para compreender a utilização desta opção [Suavização e ponderação](#suavização-e-ponderação).

### Pacote

Define o intervalo em que os dados serão agrupados durante a suavização. No arquivo padrão do Jeedom, este parâmetro é o mesmo para todos os comandos e corresponde a um múltiplo de horas. Com o archiplus, é possível especificar o intervalo para cada comando e também expressar o valor em minutos (introduza o número de minutos seguido da letra m). Consulte a seguinte FAQ para compreender a utilização desta opção [Pack](#pack-1).

### Arredondado

Por predefinição no Jeedom, é possível definir o arredondamento para cada comando. O plugin permite ainda definir um arredondamento diferente durante o alisamento dos dados no historyArch. Consulte a seguinte FAQ para compreender a utilização desta opção [Arredondamento](#arrondi-1).

## Funções acessíveis através do menu contextual

![026](../images/026.png)

Ao clicar com o botão direito do rato em qualquer ponto de uma linha da tabela, é apresentado o menu contextual do comando. Para além das ações já mencionadas, este permite:

* exibir o histórico sob a forma de gráfico (chamada da função padrão do Jeedom)
* exibir os dados armazenados nas tabelas history e historyArch
* limpar o histórico até uma determinada data
* exportar os dados históricos no formato CSV (chamada da função padrão do Jeedom)
* atualizar as estatísticas relativas à linha em questão
* iniciar o arquivamento apenas para o pedido em questão
* copiar os dados de historyArch para history: Consulte a seguinte FAQ para compreender como utilizar esta ação  [historyArch para history](#copiar-os-dados-de-historyArch-para-history)
* copiar o histórico do pedido selecionado para outro pedido

# Dados históricos

## Acesso

![027](../images/027.png)

O acesso aos dados nas tabelas history e historyArch é feito através de:

* o menu contextual do Monitor (ver acima)
* a seleção de uma ou mais linhas, seguida do premir do botão «Data».

![028](../images/028.png)

Os dados são apresentados numa janela modal, ordenados por data e hora em ordem decrescente.

## Alteração

![029](../images/029.png)

Por vezes, podem ocorrer valores anormais, neste caso devido à manutenção da caldeira.

![030](../images/030.png)

O menu contextual permite alterar ou eliminar o valor em questão.

![031](../images/031.png)

Após a correção, a visualização do histórico torna-se muito mais significativa.

## Eliminação

![032](../images/032.png)

Também é possível eliminar vários dados históricos, selecionando-os e clicando no botão «Eliminar».

## Exportação

![033](../images/033.png)

O botão «Exportar» permite exportar os dados.

Note-se que estes podem ser editados no Excel para serem importados através do módulo Import.

# O módulo Import

O módulo Import permite importar dados históricos para um ou mais comandos do tipo INFO.

![035](../images/035.png)

O ficheiro a importar deve ser do tipo Excel ou CSV e deve conter, pelo menos, as seguintes 3 colunas (as restantes serão ignoradas):

* id: ID da encomenda
* datetime: data e hora dos dados históricos no formato AAAA-MM-DD HH:MM:SS (o formato datetime interno do Excel também é suportado)
* value: valor a importar

Certifique-se de que os dados extraídos dos módulos Monitor ou Restore estão no formato correto.

![034](../images/034.png)

O primeiro passo a realizar é selecionar o ficheiro que contém os dados.

![036](../images/036.png)

Após o carregamento, os dados históricos do ficheiro são carregados.

Os dados do comando INFO são extraídos do Jeedom.

É realizada uma verificação e os dados com erros são detetados.

![037](../images/037.png)

É possível atribuir as linhas carregadas a outro comando, selecionando a(s) linha(s) em questão e clicando no botão «Alterar comando».

![038](../images/038.png)

Para importar os dados históricos para o Jeedom, é necessário selecionar a(s) linha(s) em questão (neste caso, filtrar por um intervalo de datas) e clicar no botão «Importar». As linhas com erros são ignoradas.

![039](../images/039.png)

Note-se que a importação é realizada através do método padrão cmd::addHistoryValue. Assim, são executados os controlos e processamentos padrão do Jeedom. As novas entradas encontram-se na tabela history.

# O módulo Restore

O módulo Restore permite extrair dados históricos de um arquivo padrão do Jeedom e exportá-los, para que possam ser importados com o módulo Import.

Todos os processamentos são efetuados localmente no navegador da Web. Todos os comandos e dados históricos são carregados na memória do navegador. O programa foi testado com 1,5 milhões de linhas no «history» e no «historyArch». O número máximo de dados carregados depende da memória RAM atribuída ao navegador e não pode ser conhecido antecipadamente. No entanto, deverá ser capaz de carregar a maioria das cópias de segurança em que o histórico não tenha atingido limites excessivos.

![040](../images/040.png)

O primeiro passo é recuperar a cópia de segurança localmente no computador. Consulte a seguinte documentação sobre a gestão de cópias de segurança do Jeedom [aqui](https://doc.jeedom.com/fr_FR/core/4.5/backup).

![041](../images/041.png)

Inicie o módulo Restore e selecione o arquivo que pretende utilizar.

![042](../images/042.png)

Após alguns segundos, são apresentados os comandos com o histórico.

![043](../images/043.png)

Pode selecionar os comandos que lhe interessam e iniciar a exportação.

![044](../images/044.png)

Também pode visualizar os dados históricos relevantes e selecionar aqueles que pretende exportar.

![045](../images/045.png)

Em ambos os casos, encontrará um ficheiro de exportação que pode utilizar para efetuar uma importação com o módulo Import.


![073](../images/073.png)

Ao clicar nos botões de visualização, é possível visualizar os parâmetros dos comandos INFO tal como estavam no momento em que foram guardados. O filtro «Todos» permite visualizar todos os comandos INFO.

O botão «Exportar» permite gerar um ficheiro que poderá ser utilizado para carregar no módulo «Monitor» as diferenças de configuração em relação à cópia de segurança.

# Perguntas frequentes

## Manter o último valor

Em alguns casos, é necessário dispor do último valor do comando INFO.

![046](../images/046.png)

Tomemos o caso de uma caldeira cujo contador de gás destinado ao aquecimento é lido periodicamente.

![047](../images/047.png)

Um cenário executado de hora a hora permite calcular o consumo horário, calculando a diferença entre o valor registado no histórico no início e no final da hora. Para tal, basta um histórico de um dia.

No entanto, quando a época de aquecimento termina, o histórico do contador de aquecimento desaparece e deixa de estar disponível para calcular o primeiro consumo horário aquando do primeiro aquecimento da época seguinte.

A ativação da opção «Keep Last Value» permite resolver este problema sem ter de recorrer a artifícios de programação ou manter um histórico ao longo de um ano.

## Uniq

O Jeedom permite evitar duplicados na tabela «history» com a opção «Repetir valores idênticos», que está desativada por predefinição.

No entanto, existem várias situações em que os valores consecutivos idênticos não são ignorados:

  * se o subtipo do comando for Binário ou Outro
  * se a atualização for efetuada com o método cmd::event e não com o eqLogic::checkAndUpdateCmd. Muitos plugins ainda funcionam com o método cmd::event, que é mais antigo e, por isso, não elimina as duplicatas.

Durante o arquivamento, se não houver suavização, os dados do histórico são transferidos diretamente para o historyArch e, por conseguinte, os duplicados são copiados.

A ativação da opção Uniq permite eliminar os duplicados no historyArch durante o arquivamento específico do archiplus.

Além disso, se o plugin estiver configurado para não copiar os registos do «history» para o «historyArch», as entradas duplicadas no «history» serão também eliminadas.

## Prazo e âmbito

Por predefinição, o momento a partir do qual os dados são eliminados do history e do historyArch é definido pelo parâmetro «Limpar histórico», expresso em horas. Está definido um valor por predefinição na configuração global do Jeedom.

Assim, com uma limpeza definida para 7 dias, se o arquivamento for iniciado a 20/01/2025 às 05:11:21, os registos «history» e «historyArch» serão eliminados até 13/01/2025 às 05:11:21.

O parâmetro «Cadragem», específico do Archiplus, permite definir com maior precisão o momento da purga. Assim, no exemplo acima, o momento da purga será:

* a 13/01/2025 às 05:11:21, caso não esteja definido qualquer enquadramento
* a 13/01/2025 às 05:11:00, com destaque para o último minuto
* a 13/01/2025 às 05:00:00, com um foco na última hora
* a 13/01/2025 às 00:00:00, com destaque para o último dia

O «Prazo antes do arquivo» (em horas) permite determinar a partir de que momento os registos do histórico são transferidos para o historyArch (com ou sem consolidação). Por predefinição, é definido globalmente e, por isso, é idêntico para todos os comandos.

O arquivo específico do archiplus permite definir um prazo específico para cada comando INFO e utilizar o enquadramento acima referido. Assim, com um prazo de 2 horas, o momento da transferência de history para historyArch será:

* a 20/01/2025 às 03:11:21, caso não esteja definido qualquer enquadramento
* a 20/01/2025 às 03:11:00, com destaque para o último minuto
* a 20/01/2025 às 03:00:00, com um foco na última hora
* a 20/01/2025 às 00:00:00, com um filtro para o último dia, independentemente da hora do dia em que o arquivamento for iniciado

Note-se que o momento da limpeza não pode ser posterior ao momento da transferência do history para o historyArch e, por isso, será ajustado automaticamente.

![048](../images/048.png)

É possível ajustar estes parâmetros se, por exemplo, se pretender um histórico detalhado de um período curto (neste caso, no máximo 36 horas) sem necessidade de um arquivo consolidado. Desta forma, evita-se a transferência do histórico para o «historyArch», o que não traz qualquer benefício.

## Suavização e ponderação

A suavização ocorre durante a cópia dos dados do histórico para o historyArch. O processo de arquivamento considera todos os dados do histórico de acordo com o intervalo definido (por predefinição, uma hora) e mantém um único valor calculado de acordo com o modo de suavização. São possíveis três modos:

* mínimo: o menor dos valores contidos no intervalo
* máximo: o maior dos valores contidos no intervalo
* média: a média dos valores contidos no intervalo

É importante referir que o registo padrão não tem em conta o valor do comando no início do intervalo e calcula a média dos valores presentes nesse intervalo, o que pode distorcer significativamente o resultado.

O processo específico de arquivamento do archiplus oferece uma opção «Pond» que permite corrigir este fenómeno e calcular um resultado exato para o intervalo em questão.

Isto é ilustrado no exemplo abaixo.

![050](../images/050.png)

Consideremos dois comandos com as seguintes configurações.

![049](../images/049.png)

Ambas têm as mesmas entradas na tabela «history»

![051](../images/051.png)

Após o arquivamento, as entradas no historyArch são diferentes

![052](../images/052.png)

Com o arquivo padrão, é considerada a média dos valores ao longo do período.

Com o arquivamento específico do archiplus, é calculada a média ponderada do período. Note-se também que é adicionada uma entrada no histórico para que, no próximo arquivamento, se possa conhecer o valor inicial do período (sem esta entrada, recuperar-se-ia a média do último período, o que distorceria o cálculo).

## Pacote

Por predefinição no Jeedom, o intervalo (denominado «pacote» no Jeedom) sobre o qual é possível aplicar a suavização é definido em horas e é o mesmo para todos os comandos INFO.

No entanto, pode ser desejável um intervalo mais reduzido e poder especificá-lo para um comando INFO específico.

![055](../images/055.png)

![054](../images/054.png)

No caso de uma bateria, manter um valor por dia durante um longo período pode ser suficiente.

![057](../images/057.png)

![056](../images/056.png)

No caso de um termómetro, uma leitura a cada quarto de hora pode ser mais útil do que uma leitura por hora.

Para indicar minutos, introduza na zona «Pack» o número de minutos pretendido seguido de «m», por exemplo, 15m.

## Arredondado

Por predefinição, o Jeedom permite especificar o número de casas decimais de um valor de comando INFO.

Para certos comandos, pode ser interessante dispor de um valor preciso durante um curto período de tempo e, posteriormente, de um valor menos preciso. Por exemplo, saber a temperatura exterior exata é importante no momento, mas deixa de ser necessário após vários dias.

![064](../images/064.png)

O comando acima está configurado para manter um histórico com 1 casa decimal durante uma semana e um histórico sem casas decimais para além desse período.

![065](../images/065.png)

Antes do arquivo, temos 7 registos no histórico entre 7,7 °C e 8,3 °C.

![066](../images/066.png)

Após o arquivo, os 7 valores introduzidos são arredondados para 8 °C e a opção «Uniq» permite conservar apenas um deles.

## Copiar os dados de historyArch para history

Depois de instalar o archiplus, poderá querer consolidar os registos históricos existentes.

![060](../images/060.png)

![058](../images/058.png)

Por exemplo, para este comando, um histórico com intervalos de 10 minutos seria suficiente e reduziria significativamente o número de registos no historyArch.

![059](../images/059.png)

Depois de alterar a configuração, é possível transferir os registos do historyArch para o history.

![061](../images/061.png)

Depois de efetuada esta atualização, é possível iniciar um arquivamento através deste comando INFO (ou aguardar que o arquivamento seja iniciado automaticamente durante a noite).

![063](../images/063.png)

![062](../images/062.png)

Após o arquivamento, o número de registos é significativamente reduzido e a visualização do histórico é muito mais rápida.

## Utilizar o Archiplus em PHP

É possível aceder às funções de arquivo e de processamento dos históricos do archiplus diretamente num cenário ou numa função PHP.

![053](../images/053.png)

Aqui, as funções do archiplus são utilizadas num cenário para carregar o histórico de uma encomenda e iniciar o arquivo da mesma.

`require_once dirname(__FILE__) . '../../../plugins/archiplus/core/class/archiplus.class.php';`

Esta linha permite carregar o código das funções do Archiplus. Pode ser necessário ajustar o caminho para apontar para a classe do plugin.

As funções disponíveis podem ser encontradas no código da classe archiplus. As principais são:

* `archive($_cmd_id = '')`: inicia o arquivamento de um pedido ou de todos os pedidos, caso não haja nenhum parâmetro
* `History_purge($_cmd_id, $_date='')`: elimina o histórico de um comando até uma data e hora determinadas (ou todo o histórico, se não for fornecido um segundo parâmetro)
* `addHistoryValue($_cmd_id, $_datetime, $_value)`: adiciona uma entrada (ou substitui a entrada existente) no histórico, chamando a função padrão do Jeedom
* `historyArch2history($_cmd_id, $_date_start = '', $_date_end = '')`: transfere os registos do historyArch para o history
  
É, evidentemente, possível utilizar as funções disponíveis na classe history.class.php após ter efetuado a declaração `require_once` necessária.

# Os registos

Se o nível de registo na configuração do plugin estiver definido, pelo menos, em «Info», os diversos eventos relacionados com o Archiplus serão registados no registo do Archiplus do Jeedom. É possível aceder diretamente a este registo através do botão «Registo» presente nos diversos módulos do Archiplus.

![068](../images/068.png)

Durante o arquivamento, são apresentadas as definições gerais de arquivamento do Jeedom.

![067](../images/067.png)

Em seguida, são detalhadas, para cada comando, as operações realizadas e o número de registos nos registos «history» e «historyArch» antes e depois do mesmo.

![069](../images/069.png)

É possível visualizar o registo de um comando específico, indicando o seu número precedido pelos caracteres «-» e um espaço na área de pesquisa.

# Tradução

A interface, as mensagens enviadas nos registos e a documentação estão traduzidas para as 5 línguas suportadas pelo Jeedom (obrigado ao @mips pelo desenvolvimento do ga-translation e do docs-translations). Se forem detetados erros de tradução, pode abrir um pedido de suporte e, se possível, anexar o ficheiro de tradução corrigido (localizado no diretório core/i18n do plugin).

# Opiniões

![archiplus_opinião](../images/archiplus_avis.png)

Se gostar deste plugin, por favor, deixe uma avaliação e um comentário no Jeedom Market, é sempre um prazer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4679#>
