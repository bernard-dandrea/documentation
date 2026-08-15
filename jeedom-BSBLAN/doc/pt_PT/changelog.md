# Registo de alterações do plugin BSBLAN

# 28/07/2026

- Encriptação dos códigos de acesso na base de dados
- Melhoria na gestão das tarefas cron
- Possibilidade de executar o cron no motor de tarefas
- Limpeza de código

# 05/01/2026

- Substituição de «event» por «checkAndUpdateCmd» para evitar a repetição de valores no histórico
- Transferência da documentação para um repositório GitHub separado, para que seja possível atualizar a documentação sem ter de atualizar o plugin

# 27/01/2025

- Gestão de comandos de atualização através de JSON ou URL /S (ver documentação)

# 10/11/2024

- Atualização da documentação

# 07/11/2024

- Conversão dos métodos cron para estáticos, para evitar erros no PHP 8

# 06/08/2024

- Possibilidade de submeter várias vezes uma encomenda que tenha falhado

Após a atualização para o Debian 11, verifiquei que estava a obter erros de tempo limite após enviar comandos para o BSBLAN (isto não acontecia no Debian 10 e não sei onde procurar para resolver o problema ao nível do sistema operativo). Ao enviar o comando novamente, este é geralmente executado sem problemas. Por isso, adicionei a cada equipamento uma opção «Número de tentativas» que permite enviar o comando várias vezes.

# 28/04/2024

- Pequena atualização da documentação

# 25/02/2024

- Aumento do número de caracteres dos nomes de comando de 40 para 100
- Atualização da documentação

# 28/12/2023

- Adicionar um comando de atualização (este é criado quando se guarda o equipamento)
  
# 21/10/2023

- Atualizar mensagem de depuração
- Atualização do índice e do registo de alterações para a versão beta

# 01/08/2023

- Adicionar um tempo limite para as solicitações HTTP

# 10/07/2023

- Carregamento inicial

