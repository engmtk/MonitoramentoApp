Monitoramento de Execuções
Este projeto é um sistema de Monitoramento de Execuções que acompanha e gerencia o processamento de fluxos em um banco de dados x a aplicação que executa supostamente em um site interno pela área de negócio. 
A aplicação permite que o time de tecnologia visualize o status das execuções realizadas pelo usuário no site interno em tempo real, com a possibilidade de realizar ações manuais, como remover da fila, retornar para a fila e reiniciar fluxos específicos.
Importante: O funcionamento correto do sistema depende do Robô Execution, uma aplicação do tipo console que executa as procedures e atualiza a tabela TBy9_LogExecucao. Sem ele, o site não terá dados para exibir ou gerenciar.
Funcionalidades Principais

1. Exibição das Execuções

As execuções são listadas em uma tabela, mostrando detalhes como:
ID da Execução
Nome da Procedure
Data de Início e Fim
Status Atual
Duração (segundos)
Observações
O sistema exibe as 20 execuções mais recentes no topo, garantindo acesso rápido às execuções mais relevantes.

2. Ações Manuais
Remover da Fila: Remove a execução do fluxo, impedindo o processamento, mas registrando a ação no histórico.
Retornar para Fila: Recoloca a execução removida de volta na fila, permitindo que o Robô Execution a processe novamente.
Reiniciar Fluxo: Reinicia execuções com erro ou em andamento, registrando a ação no campo Observação.

3. Histórico de Ações
O campo Observação mantém um registro detalhado de todas as ações realizadas, como:
Retirado da fila manualmente
Retornado para fila manualmente
Fluxo reiniciado manualmente
Isso garante rastreabilidade total das execuções.

4. Atualizações em Tempo Real
Utiliza SignalR para exibir mudanças instantâneas nas execuções.
Um contador regressivo de 60 segundos no canto superior direito indica o tempo para o próximo recarregamento automático.

5. Log de Ações
As interações são registradas em um log detalhado, facilitando auditorias e verificações.

Tecnologias Utilizadas
ASP.NET Core MVC: Estrutura principal do site.
C#: Backend para lógica de negócios.
SQL Server: Banco de dados das execuções.
SignalR: Atualizações em tempo real.
HTML/CSS/JavaScript: Interface do usuário.
Bootstrap: Estilo responsivo.
EPPlus: Exportação para Excel.

Como Funciona o Sistema

1.	O Robô Execution executa as procedures e atualiza a tabela TBy9_LogExecucao no banco CadastroDB.
2.	O site exibe essas execuções e permite ações como remover, retornar ou reiniciar.
3.	As alterações são refletidas automaticamente na interface em tempo real.
4.	Logs detalhados são registrados para auditoria.

Explicação sobre a Procedure e o Robô Execution

Antes das Alterações:
1.	A cada execução, a procedure criava uma nova entrada no log com o status Executando.
2.	Não verificava se já havia execuções pendentes (Em Fila).
3.	Nenhuma observação era registrada automaticamente.

Após as Alterações:

1.	Processamento de Execuções Pendentes:
Se houver uma execução Em Fila, a procedure:
Atualiza o status para Executando.
Registra a observação Retomado pelo robô.
Ao concluir, marca como Concluída com a observação Concluída pelo robô.

2.	Histórico Detalhado:
As observações agora incluem:
Retomado pelo robô
Concluída pelo robô
APROV_AUTOMATICA para execuções automáticas.

3.	Criação de Novas Execuções:
A procedure só cria uma nova execução se não houver pendências.
A nova execução é criada com a observação APROV_AUTOMATICA.
Alguns ajustes
Gerenciamento Eficiente: Prioriza execuções pendentes, evitando duplicidade.
Rastreamento Completo: O histórico mantém todas as ações do usuário e do robô.
Identificação Clara: Execuções automáticas são marcadas com APROV_AUTOMATICA.

Execução do Projeto
1.	Inicie o Robô Execution (aplicativo console).
2.	Execute o site MonitoramentoApp no Visual Studio.
3.	Acesse https://localhost:5001 para visualizar as execuções em tempo real.

Nota: Sem o Robô Execution, o site não receberá atualizações, pois é o robô que executa as procedures e alimenta a base de dados.

