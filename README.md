# guia-estudos-notebookLM
Desafio proposto de explorar as capacidades da Inteligência Artificial do NotebookLM afim de explorar um campo de estudo.

Escolhi algo relacionado a dados visto que é meu campo de interesse profissional.

Primeiro, inseri três fontes que encontrei relacionado a SQL, Performance e BI, que são:

https://learn.microsoft.com/pt-br/sql/t-sql/language-reference?view=sql-server-ver17

https://www.researchgate.net/publication/390897446_Performance_Tuning_in_Power_BI_and_SQL_Enhancing_Query_Efficiency_and_Data_Load_Times

https://www.mdpi.com/2227-7099/11/3/99

primeiro comando: "Explique como funciona a otimização de consultas no SQL Server e quais fatores mais impactam a performance." o resultado obtido foi:

No SQL Server, a otimização de consultas é o processo pelo qual o mecanismo de banco de dados decide a maneira mais eficiente de executar um comando Transact-SQL (T-SQL).
Como todas as ferramentas e aplicativos se comunicam com o banco enviando esses comandos, a forma como eles são processados é fundamental para o desempenho.
De acordo com a documentação, os principais fatores e ferramentas que impactam a performance são:
Índices (Indexes): São essenciais tanto no design básico de tabelas quanto na etapa de ajuste (tuning) para acelerar a busca de dados.


Estatísticas (Statistics): Informações que ajudam o otimizador a entender a distribuição dos dados e escolher o melhor caminho de execução.

Design de Consulta (T-SQL): A estrutura de instruções como SELECT, INSERT, UPDATE e DELETE impacta diretamente como o servidor processa a requisição.

Ferramentas de Ajuste: O SQL Server oferece recursos como o Ajuste Automático, o Orientador de Ajuste de Banco de Dados e o Repositório de Consultas (Query Store) para monitorar e sugerir melhorias em consultas lentas

Recursos Avançados: O uso de OLTP na memória e o gerenciamento correto do log de transações também são citados como elementos críticos para a administração da performance.


Em seguida, aproveitei o gancho da conversa e utilizei de uma opção sugerida pelo próprio NotebookLM para seguir a conversa: "Como os Índices ajudam na otimização?" e como resultado:

Os índices funcionam como um mapa para os dados, permitindo que o SQL Server localize informações rapidamente sem precisar ler todas as linhas de uma tabela. O uso de índices bem projetados pode reduzir o tempo de execução das consultas em 60% ou mais.

Eles ajudam de três formas principais:
Agilidade na Busca: Índices agrupados (Clustered) organizam os dados fisicamente, enquanto os não agrupados (Non-Clustered) criam estruturas separadas para colunas consultadas com frequência.

Performance em Consultas Complexas: Índices compostos (que combinam várias colunas) são especialmente eficazes para filtros complexos, podendo melhorar o desempenho em até 50%.

Eficiência de Recursos: Ao localizar dados mais rápido, o sistema consome menos CPU e memória durante o processamento.

Para manter esses ganhos, é fundamental realizar auditorias e manutenções regulares, garantindo que os índices não percam eficiência conforme o volume de dados aumenta.

Após a explicação sobre índices, resolvi fazer uma pergunta comparativa "Qual a diferença de performance entre usar CTEs e tabelas temporárias em T-SQL?", e como resultado:

Embora as fontes não comparem diretamente CTEs e tabelas temporárias em um cenário de "um contra o outro", elas fornecem dados sobre as técnicas que tornam cada uma eficiente:
CTEs (Common Table Expressions): São excelentes para a legibilidade e para o que a literatura chama de "subquery flattening" (achatamento de subconsultas) e reescrita de consultas. Essas técnicas avançadas de otimização podem reduzir o tempo de execução em até 40%. Como são tratadas como parte da consulta principal, elas se beneficiam diretamente da análise do plano de execução feito pelo otimizador.

Tabelas Temporárias: São mais vantajosas quando você precisa de recursos físicos que as CTEs não oferecem, como a criação de índices ou particionamento. As fontes indicam que o uso de índices apropriados pode melhorar a performance em mais de 60%, enquanto o particionamento de tabelas grandes pode reduzir o tempo de resposta em até 70%.

Em termos práticos, se a sua lógica for complexa e exigir múltiplos filtros ou reutilização de dados em tabelas muito grandes, as tabelas temporárias permitem o uso de índices que aceleram drasticamente a busca. Para transformações de lógica simples e melhor organização do código, as CTEs são a escolha padrão para otimização de consultas.

E por fim, solicitar um pequeno exemplo aplicando conceitos presentes nas fontes: "Mostre um exemplo de query mal otimizada e sua versão otimizada." que me retornou:

Com base nas técnicas de reescrita de consulta (query rewriting) e achatamento de subconsultas (subquery flattening) discutidas nas fontes, aqui está um exemplo de como transformar uma consulta ineficiente em uma otimizada:

Versão Mal Otimizada
Neste exemplo, a consulta utiliza uma subconsulta (que pode ser lenta em grandes volumes de dados) e solicita todas as colunas desnecessariamente.
-- Seleciona todas as colunas e usa uma subconsulta "IN"
SELECT * 
FROM Clientes 
WHERE IdCliente IN (SELECT IdCliente FROM Vendas WHERE DataVenda > '2024-01-01');

Versão Otimizada
Aqui, aplicamos a reescrita da consulta para um JOIN e selecionamos apenas as colunas essenciais.
-- Uso de JOIN e seleção específica de colunas (Query Rewriting)
SELECT DISTINCT c.Nome, c.Email 
FROM Clientes c
INNER JOIN Vendas v ON c.IdCliente = v.IdCliente
WHERE v.DataVenda > '2024-01-01';

Por que a versão otimizada é melhor?
Reescrita de Consulta (Query Rewriting): O uso de técnicas avançadas como transformar subconsultas em JOINs pode reduzir o tempo de execução em até 40%.
Redução de Volume: Ao selecionar apenas Nome e Email em vez de *, você diminui a carga de processamento e o uso de memória.
Uso de Índices: Se houver um índice na coluna DataVenda, a performance de recuperação de dados pode melhorar em mais de 60%.
Particionamento: Caso a tabela Vendas seja muito grande e esteja particionada por data, o SQL Server lerá apenas a partição relevante, o que pode acelerar a busca em até 70%.

No Power BI, essa mesma lógica se aplica: evitar colunas desnecessárias no modelo de dados pode melhorar o tempo de carregamento dos relatórios em 30%.


Resumo do que foi aprendido:

Índices são estruturas que funcionam como um “índice de livro”: eles permitem que o banco encontre dados muito mais rápido, sem precisar varrer toda a tabela, criar índices nas colunas usadas pode reduzir drasticamente o tempo de execução das queries.
Relação com performance de queries
Sem índice -> o banco lê a tabela inteira (lento)
Com índice -> o banco vai direto ao ponto (rápido)

CTEs são úteis para organizar e simplificar consultas complexas. Elas não armazenam dados fisicamente (são temporárias em memória durante a execução), além de ajudar na performance, ajuda na legibilidade e entendimento do código.

As tabelas temporárias  armazenam dados temporariamente no banco, além de poder ter índices, é útil trabalhar com elas quando se trata de um grande volume de dados, pois evitam reprocessamento.
