# uvv_bd_1_si1n

# pset 2

## questão 1

SELECT 
  numero_departamento AS departamento
, AVG(salario)        AS media_salarial
FROM elmasri.funcionario
GROUP BY numero_departamento;

## questão 2
SELECT 
  sexo
, AVG(salario) AS media_salarial
FROM elmasri.funcionario
GROUP BY sexo;

## questão 3

SELECT 
  nome_departamento
, CONCAT(primeiro_nome,' ', nome_meio,' ', ultimo_nome)                                 AS nome_completo_funcionario
, data_nascimento, DATE_PART('year', CURRENT_DATE) - DATE_PART('year', data_nascimento) AS idade
, salario
FROM elmasri.funcionario        f
INNER JOIN elmasri.departamento dp ON (f.numero_departamento = dp.numero_departamento);

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





