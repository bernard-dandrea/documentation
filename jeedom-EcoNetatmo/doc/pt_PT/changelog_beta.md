# Registo de alterações do plugin EcoNetatmo

# 05/08/2026

- tradução do plugin e da documentação para en_US, es_ES, de_DE, it_IT, pt_PT
- mudança para Vanilla JS
- versão mínima do Jeedom -> 4.4.0
- limpeza de código e otimizações

# 22/07/2026

- Possibilidade de definir um cron autónomo no motor de tarefas

# 07/11/2025

- Correção de um problema no PHP 8 durante a renovação dos tokens
- Correção para eliminar o aviso do PHP durante a sincronização
- Transferência da documentação para um repositório GitHub separado, para que seja possível atualizar a documentação sem ter de atualizar o plugin
- Validação do plugin no Debian 12 Jeedom 4.5
  
# 09/09/2025

- Correção dos endereços de Internet da Netamo (substituir .net por .com): será certamente necessário regenerar os tokens para restabelecer a comunicação

# 20/05/2025

- Alteração da biblioteca de acesso ao Netatmo na sequência de um problema que impedia a recuperação de dados

# 07/11/2024

- Conversão dos métodos cron para estáticos, para evitar erros no PHP 8

# 25/02/2024

- Atualização da documentação

# 21/07/2023

- Remoção do parâmetro «scope» durante a atualização do token

# 20/07/2023

- Alteração para gerir a autenticação através do authorization_code (ver documentação)

# 26/05/2023

- Remoção do link do PayPal
- Alteração dos links para a documentação no GitHub
- Adicionar ligações para a documentação beta

# 25/05/2023

- Carregamento inicial
