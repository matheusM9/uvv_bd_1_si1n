# uvv_bd_si1n
## pset 1 
## matheus silva santana 

Repositorio criado para documentar o pset1 e a sequencia de ações feitas

## primeira sequência de ações
entendi melhor como funciona o github, abri o repositorio e sub diretorio, aprendi o basico da linguagem markdown e na sequencia respondi as questões 1,2 e3
## segunda sequência de ações
a maquina virtual nao funcionau muito bem no meu notebook, deixou lento e travou, baixei o power architect, postgresql e o mysql.
estrurei o projeto do elmasri no power architect, criação de tabelas e relacionamentos, apos respondi a questoes 4 a 9.
## terceira sequência de acoes 
inseri no script os comentarios das tabelas e linhas, utilizei um arquivo no bloco de notas para testar e corrigir o script
abri database uvv
create database uvv;
criei o schema elmasri 
create schema elmasri;
defini o schema elmasri como principal
set search_path to elmasri;

## quarta sequência de ações 
inseri o projeto no postgresql  com os comentarios  sem nenhum erro apontado pelo software problemas, mas na hora de inserir os dados na tabelas nao foi nenhum comando, nao tive exito em colocar os dados. 
exemplo de erro: comando - insert into elmasri.dependente(cpf_funcionario,nome_dependente,sexo,data_nascimento,parentesco)
                           values (33344555587.alicia,f,'05-04-1986');
                           erro:colunm ''alicia'' does not exist. 
posteriormento respondi as questoes 10,11,12
## ultima sequência de ações 
voltei ao power arquitect copiei o script do mysql,criei o database uvv, implementei no mysql.
respondi as questoes 14 e 15. 

# script postgresql
CREATE TABLE elmasri.funcionario (
                cpf CHAR(11) NOT NULL,
                primeiro_nome  VARCHAR(15) NOT NULL,
                nome_meio CHAR(1),
                ultimo_nome VARCHAR(15) NOT NULL,
                data_nascimento DATE,
                endereco VARCHAR(30),
                sexo CHAR(1),
                salario NUMERIC(10,2),
                cpf_supervisor CHAR(11) NOT NULL,
                numero_departamento INTEGER NOT NULL,
                CONSTRAINT funcionario_pk PRIMARY KEY (cpf)
);


CREATE TABLE elmasri.trabalha_em (
                cpf_funcionario CHAR(11) NOT NULL,
                numero_projeto integer not null,
                horas NUMERIC(3,1) NOT NULL,
                CONSTRAINT trabalha_em_pk PRIMARY KEY (cpf_funcionario)
);


CREATE TABLE elmasri.dependente (
                cpf_funcionario CHAR(11) NOT NULL,
                nome_dependente VARCHAR(15) NOT NULL,
                sexo CHAR(1),
                data_nascimento DATE,
                parentesco VARCHAR(15),
                CONSTRAINT dependente_pk PRIMARY KEY (cpf_funcionario, nome_dependente)
);


CREATE TABLE elmasri.departamento (
                numero_departamento INTEGER NOT NULL,
                nome_departamento VARCHAR(15) NOT NULL,
                cpf_gerente CHAR(11) NOT NULL,
                data_inicio_gerente DATE,
                CONSTRAINT departamento_pk PRIMARY KEY (numero_departamento)
);


CREATE UNIQUE INDEX departamento_idx
 ON elmasri.departamento
 ( nome_departamento );

CREATE TABLE elmasri.projeto (
                numero_projeto INTEGER NOT NULL,
                nome_projeto VARCHAR(15) NOT NULL,
                local_projeto VARCHAR(15) NOT NULL,
                numero_departamento INTEGER NOT NULL,
                CONSTRAINT projeto_pk PRIMARY KEY (numero_projeto)
);


CREATE UNIQUE INDEX projeto_idx
 ON elmasri.projeto
 ( nome_projeto );

CREATE TABLE elmasri.localizacoes_departamento (
                numero_departamento INTEGER NOT NULL,
                local VARCHAR(15) NOT NULL,
                CONSTRAINT localizacoes_departamento_pk PRIMARY KEY (numero_departamento, local)
);


ALTER TABLE elmasri.funcionario ADD CONSTRAINT funcionario_funcionario_fk
FOREIGN KEY (cpf_supervisor)
REFERENCES elmasri.funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE elmasri.departamento ADD CONSTRAINT funcionario_departamento_fk
FOREIGN KEY (cpf_gerente)
REFERENCES elmasri.funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE elmasri.dependente ADD CONSTRAINT funcionario_dependente_fk
FOREIGN KEY (cpf_funcionario)
REFERENCES elmasri.funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE elmasri.trabalha_em ADD CONSTRAINT funcionario_projeto_fk
FOREIGN KEY (cpf_funcionario)
REFERENCES elmasri.funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE elmasri.localizacoes_departamento ADD CONSTRAINT departamento_localizacoes_departamento_fk
FOREIGN KEY (numero_departamento)
REFERENCES elmasri.departamento (numero_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE elmasri.projeto ADD CONSTRAINT departamento_projeto_fk
FOREIGN KEY (numero_departamento)
REFERENCES elmasri.departamento (numero_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

COMMENT ON TABLE elmasri.departamento IS 'tabela que armazena as informacoes dos departamentos.';
COMMENT ON TABLE elmasri.dependente IS 'tabela que armazena as informacoes dos dependentes dos funcionarios.';
COMMENT ON TABLE elmasri.funcionario IS 'tabela que armazena as informacoes dos funcionarios.';
COMMENT ON TABLE elmasri.localizacoes_departamento IS 'tabela que armazena as possiveis localizacoes dos departamentos.';
COMMENT ON TABLE elmasri.projeto IS 'tabela que armazena as informacoes sobre os projetos dos departamentos.';
COMMENT ON TABLE elmasri.trabalha_em IS 'tabela para armazenar quais funcionarios trabalham no projeto.';


COMMENT ON COLUMN elmasri.departamento.numero_departamento IS'Numero do departamento. e a PK desta tabela.';COMMENT ON COLUMN elmasri.departamento.nome_departamento IS 'nome do departamento. deve ser unico.';
COMMENT ON COLUMN elmasri.departamento.cpf_gerente IS 'cpf do gerente do departamento. e uma fk para a tabela funcinarios.';
COMMENT ON COLUMN elmasri.departamento.data_inicio_gerente IS 'data do inicio do gerente no departamento.';
COMMENT ON COLUMN elmasri.departamento.nome_departamento IS 'nome do departamento. deve ser unico.';

COMMENT ON COLUMN elmasri.dependente.cpf_funcionario IS 'cpf do funcionario.faz parte desta tabela e e uma fk para a  tabela funcionario.';
COMMENT ON COLUMN elmasri.dependente.nome_dependente IS 'nome do dependente.faz parte da pk desta tabela.';
COMMENT ON COLUMN elmasri.dependente.sexo IS 'sexo da pessoa.';
COMMENT ON COLUMN elmasri.dependente.data_nascimento IS 'data de nascimento.';
COMMENT ON COLUMN elmasri.dependente.parentesco IS 'descricao de parentesco com funcionario.';


COMMENT ON COLUMN elmasri.funcionario.primeiro_nome IS 'primeiro nome do funcionario.';
COMMENT ON COLUMN elmasri.funcionario.nome_meio IS 'nome do meio.';
COMMENT ON COLUMN elmasri.funcionario.ultimo_nome IS 'sobrenome do funcionario.';
COMMENT ON COLUMN elmasri.funcionario.endereco IS 'endereco do funcionario.';
COMMENT ON COLUMN elmasri.funcionario.salario IS 'salario do funcionario.';
COMMENT ON COLUMN elmasri.funcionario.cpf_supervisor IS 'cpf do supervisor.sera uma fk para a propria tabela.';
COMMENT ON COLUMN elmasri.funcionario.numero_departamento IS 'numero do departamento.';

COMMENT ON COLUMN elmasri.localizacoes_departamento.local IS 'localizacao do departamento.';
COMMENT ON COLUMN elmasri.localizacoes_departamento.numero_departamento IS 'numero do departamento.faz parte da pk desta tabela e tambem e uma fk para a tabela departamento.';

COMMENT ON COLUMN elmasri.projeto.numero_projeto IS 'numero projeto é o pk desta tabela.';
COMMENT ON COLUMN elmasri.projeto.nome_projeto IS 'nome do projeto.deve ser unico.';
COMMENT ON COLUMN elmasri.projeto.local_projeto IS 'localizacao do projeto.';
COMMENT ON COLUMN elmasri.projeto.numero_departamento IS 'numero do departamento.e uma fk para a tabela departamento.';

COMMENT ON COLUMN elmasri.trabalha_em.cpf_funcionario IS ' cpf do funcionario faz parte da pk desta tabela e e uma fk para a tabela funcionario.';
COMMENT ON COLUMN elmasri.trabalha_em.horas IS 'horas trabalhadas pelo funcionario neste projeto.';
COMMENT ON COLUMN elmasri.trabalha_em.numero_projeto IS 'numero do projeto faz parte desta tabela e e uma fk para a tabela projeto.';

# script mysql

CREATE TABLE funcionario (
                cpf CHAR(11) NOT NULL,
                primeiro_nome  VARCHAR(15) NOT NULL,
                nome_meio CHAR(1),
                ultimo_nome VARCHAR(15) NOT NULL,
                data_nascimento DATE,
                endereco VARCHAR(30),
                sexo CHAR(1),
                salario DECIMAL(10,2),
                cpf_supervisor CHAR(11) NOT NULL,
                numero_departamento INT NOT NULL,
                PRIMARY KEY (cpf)
);


CREATE TABLE dependente (
                cpf_funcionario CHAR(11) NOT NULL,
                nome_dependente VARCHAR(15) NOT NULL,
                sexo CHAR(1),
                data_nascimento DATE,
                parentesco VARCHAR(15),
                PRIMARY KEY (cpf_funcionario, nome_dependente)
);


CREATE TABLE departamento (
                numero_departamento INT NOT NULL,
                nome_departamento VARCHAR(15) NOT NULL,
                cpf_gerente CHAR(11) NOT NULL,
                data_inicio_gerente DATE,
                PRIMARY KEY (numero_departamento)
);


CREATE UNIQUE INDEX departamento_idx
 ON departamento
 ( nome_departamento );

CREATE TABLE projeto (
                numero_projeto INT NOT NULL,
                nome_projeto VARCHAR(15) NOT NULL,
                local_projeto VARCHAR(15) NOT NULL,
                numero_departamento INT NOT NULL,
                PRIMARY KEY (numero_projeto)
);


CREATE UNIQUE INDEX projeto_idx
 ON projeto
 ( nome_projeto );

CREATE TABLE trabalha_em (
                cpf_funcionario CHAR(11) NOT NULL,
                numero_projeto INT NOT NULL,
                horas DECIMAL(3,1) NOT NULL,
                PRIMARY KEY (cpf_funcionario, numero_projeto)
);


CREATE TABLE localizacoes_departamento (
                numero_departamento INT NOT NULL,
                local VARCHAR(15) NOT NULL,
                PRIMARY KEY (numero_departamento, local)
);


ALTER TABLE funcionario ADD CONSTRAINT funcionario_funcionario_fk
FOREIGN KEY (cpf_supervisor)
REFERENCES funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE departamento ADD CONSTRAINT funcionario_departamento_fk
FOREIGN KEY (cpf_gerente)
REFERENCES funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE dependente ADD CONSTRAINT funcionario_dependente_fk
FOREIGN KEY (cpf_funcionario)
REFERENCES funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE trabalha_em ADD CONSTRAINT funcionario_projeto_fk
FOREIGN KEY (cpf_funcionario)
REFERENCES funcionario (cpf)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE localizacoes_departamento ADD CONSTRAINT departamento_localizacoes_departamento_fk
FOREIGN KEY (numero_departamento)
REFERENCES departamento (numero_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE projeto ADD CONSTRAINT departamento_projeto_fk
FOREIGN KEY (numero_departamento)
REFERENCES departamento (numero_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE trabalha_em ADD CONSTRAINT projeto_trabalha_em_fk
FOREIGN KEY (numero_projeto)
REFERENCES projeto (numero_projeto)
ON DELETE NO ACTION
ON UPDATE NO ACTION;
                        
                        
                      


