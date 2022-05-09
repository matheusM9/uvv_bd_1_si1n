# uvv_bd_1_si1n

# pset 2

## questão 1

SELECT 
  numero_departamento AS departamento
, AVG(salario)        AS media_salarial
FROM elmasri.funcionario
GROUP BY numero_departamento;

| departamento | medial salarial |
|--- |--- |
| 5 | 33.250,00 |
| 4 | 31.000,00 |
|3  | 55.000,00 |
## questão 2
SELECT 
  sexo
, AVG(salario) AS media_salarial
FROM elmasri.funcionario
GROUP BY sexo;

| sexo | media salarial |
|--- |--- |
| M | 37.600,00 |
| F | 31.000,00 |


## questão 3

SELECT 
  nome_departamento
, CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome)                                 AS nome_completo_funcionario
, data_nascimento, DATE_PART('year', CURRENT_DATE) - DATE_PART('year', data_nascimento) AS idade
, salario
FROM elmasri.funcionario        f
INNER JOIN elmasri.departamento dp ON (f.numero_departamento = dp.numero_departamento);

| nome departamento | nome completo funcionario | data de nascimento | idade | salario|
|--- |--- |---|---|---|
| matriz | jorge e brito | 1937-11-10|85 |55.00,00|
|  pesquisa | fernando t wong|1955-12-08|67|40.000,00 |
|administração|jennifer s souza|1955-12-08|81|43.000,00|
|pesquisa|joão b silva|1965-01-09|57|30.000,00|
|pesquisa|joice a leite|1972-07-31|51|25.000,00|
|pesquisa|ronaldo k lima|1962-08-15|60|38.000,00|
|administração|alice j zelaya|1968-01-19|54|25.000,00|
|administração|andré v pereira|1969-03-29|53|25.000,00|

##quetão 4

SELECT 
   CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome)                AS nome_completo_funcionario
 , DATE_PART('year', CURRENT_DATE) - DATE_PART('year', data_nascimento) AS idade
 , salario                                                              AS salario_atual
 , CASE  
          WHEN salario < 35000 THEN salario + salario * 20/100
          WHEN salario >= 35000 THEN salario + salario * 15/100
   END                                                                  AS salario_reajuste
FROM  elmasri.funcionario;

|  nome completo funcionario  | idade | salario atual|salario reajuste|
|--- |--- |---|---|
| jorge e brito |85 |55.00,00| 63.250,00|
| fernando t wong |67|40.000,00 |46.000,00|
|jennifer s souza|81|43.000,00|49.450,00|
|joão b silva|57|30.000,00|36.000,00|
|joice a leite|51|25.000,00|30.000,00|
|ronaldo k lima|60|38.000,00|43.700,00|
|alice j zelaya|54|25.000,00|30.000,00|
|andré v pereira|53|25.000,00|30.000,00|

## questão 5 

SELECT 
  nome_departamento,
  
  CASE 
       WHEN dp.numero_departamento = 1 THEN 'Jorge'
       WHEN dp.numero_departamento = 4 THEN 'Jennifer'
       WHEN dp.numero_departamento = 5 THEN 'Fernando'
  END             AS nome_gerente
 
, primeiro_nome   AS nome_funcionario
, salario         AS salario_funcionario
FROM elmasri.departamento      dp
INNER JOIN elmasri.funcionario f ON (f.numero_departamento = dp.numero_departamento)
ORDER BY nome_departamento ASC, salario DESC;

| nome departamento |nome gerente| nome  funcionario | salario funcionario|
|--- |--- |---|---|
| administração | jennifer | jennifer|43.000,00|
| administração | jennifer|alice|25.000,00 |
|administração|jennifer |andre8|25.000,00|
|matriz|jorge|jorge|55.000,00|
|pesquisa|fernando|fernando|40.000,00|
|pesquisa|fernando|ronaldo|38.000,00|
|pesquisa|fernando|joão|30.000,00|
|pesquisa|fernando|joão|25.000,00|

## questão 6 
SELECT 
  CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome)                  AS nome_completo_funcionario
, numero_departamento                                                    AS departamento
, CONCAT(nome_dependente, ' ', ultimo_nome)                              AS nome_completo_dependente
, DATE_PART('year', CURRENT_DATE) - DATE_PART('year', d.data_nascimento) AS idade_dependente

, CASE 
       WHEN d.sexo = 'M' THEN 'Masculino'
       WHEN d.sexo = 'F' THEN 'Feminino'
  END                                                                    AS sexo_dependente
FROM elmasri.funcionario      f
INNER JOIN elmasri.dependente d ON (f.cpf = d.cpf_funcionario);

## questão 7 

SELECT DISTINCT 
 CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome) AS nome_completo_funcionario
, f. numero_departamento                               AS departamento
, salario
FROM elmasri.funcionario           f
LEFT OUTER JOIN elmasri.dependente d ON (f.cpf = d.cpf_funcionario)
WHERE cpf_funcionario IS NULL;

## questão 8

SELECT 
  f.numero_departamento                                 AS departamento
, nome_projeto
, CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome) AS nome_completo_funcionario
, SUM(horas)                                            AS horas_por_projeto -- decidi realizar a somatoria para a tabela ficar mais legivel
FROM elmasri.funcionario       f
INNER JOIN elmasri.trabalha_em t ON (f.cpf = t.cpf_funcionario)
INNER JOIN elmasri.projeto     p ON (p.numero_projeto = t.numero_projeto)
WHERE f.cpf = t.cpf_funcionario
GROUP BY departamento, nome_projeto, nome_completo_funcionario 
ORDER BY nome_completo_funcionario;

## questão 9 
SELECT 
  nome_departamento
, nome_projeto
, SUM(horas) AS soma_horas
FROM elmasri.departamento      dp
INNER JOIN elmasri.projeto     p ON (dp.numero_departamento = p.numero_departamento)
INNER JOIN elmasri.trabalha_em t ON (p.numero_projeto = t.numero_projeto)
GROUP BY nome_departamento, nome_projeto;

## questão 11


SELECT DISTINCT 
  CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome) AS nome_completo_funcionario
, nome_projeto
, SUM(horas * 50)                                       AS valor_total
FROM elmasri.funcionario       f
INNER JOIN elmasri.projeto     p   ON (f.numero_departamento = p.numero_departamento)
INNER JOIN elmasri.trabalha_em t   ON (p.numero_projeto = t.numero_projeto)
GROUP BY nome_completo_funcionario, nome_projeto
ORDER BY nome_completo_funcionario;

## questão 12 
SELECT 
  nome_departamento
, nome_projeto
, primeiro_nome AS nome_funcionario
FROM elmasri.projeto            p
INNER JOIN elmasri.departamento dp ON (p.numero_departamento = dp.numero_departamento)
INNER JOIN elmasri.funcionario  f  ON (p.numero_departamento = f.numero_departamento)
INNER JOIN elmasri.trabalha_em  t  ON (p.numero_projeto = t.numero_projeto)
WHERE t.horas IS NULL OR t.horas = 0;

## questão 13 

  SELECT 
  CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome)                AS nome_completo_pessoa
, sexo                                                                 AS sexo
, DATE_PART('year', CURRENT_DATE) - DATE_PART('year', data_nascimento) AS idade
FROM elmasri.funcionario
UNION
SELECT CONCAT(nome_dependente, ' ', f.ultimo_nome)
, d.sexo
, DATE_PART('year', CURRENT_DATE) - DATE_PART('year', d.data_nascimento)
FROM elmasri.dependente        d
INNER JOIN elmasri.funcionario f ON (cpf_funcionario = cpf)
ORDER BY idade DESC;

## questão 14

SELECT 
  nome_departamento AS departamento 
, COUNT(cpf)        AS numero_de_funcionarios
FROM elmasri.funcionario        f
INNER JOIN elmasri.departamento dp ON (f.numero_departamento = dp.numero_departamento)
GROUP BY nome_departamento;

## questão 15

SELECT 
  CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome) AS nome_completo_funcionario
, f.numero_departamento                                 AS departamento
, nome_projeto                                          AS nome_projeto_alocado
FROM elmasri.funcionario        f
LEFT OUTER JOIN elmasri.projeto p ON (f.numero_departamento = p.numero_departamento);





