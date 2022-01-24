# PERGUNTA_12
CREATE OR REPLACE PACKAGE om_pkg_tas IS
  PROCEDURE TAREFA (xSTATUS IN EQUIPES.cNOME%TYPE);
END;
--CREATE PACKAGE SE PRECISA O STATUS
CREATE OR REPLACE PACKAGE om_pkg_task is
 procedure TAREFA (xSTATUS EQUIPES.cNOME%TYPE)
  IS
    xSTATUS EQUIPES.cNOME%TYPE;
   
--CREATE TABLE EQUIPE PARA ARMAZENAR OS REGISTROS
CREATE TABLE EQUIPES(
  oid INTEGER PRIMARY KEY AUTOINCREMENT, 
  nome VARCHAR (20),
  nome_b1 VARCHAR (20),
  nome_b2 VARCHAR (20),
  nome_b3 VARCHAR (20),
  status INT
  );
 
 --CREATE TABLE CABECALHO_TAREFA PARA ARMAZENAR OS REGISTROS
 CREATE TABLE CABECALHO_TAREFA(
   oid INTEGER PRIMARY KEY AUTOINCREMENT, 
   nome VARCHAR (20),
   data_criacao VARCHAR (20),
   área VARCHAR (30),
   equipe_responsável INT --(número da equipe cadastrada na tabela anterior)
   );
 
 --CREATE TABLE LOG_PROCESSO PARA ARMAZENAR OS REGISTROS
   CREATE TABLE  LOG_PROCESSO(
     oid INTEGER PRIMARY KEY AUTOINCREMENT, 
     data DATE, 
     codigo INT,
     descricao VARCHAR (30)
     );
  
  -- INSERIR OS DADOS A TABELA EQUIPES     
        INSERT INTO EQUIPES (nome,nome_b1,nome_b2,nome_b3,status) VALUES 
     ('ALPHA1','MT_07019','13TRF','E08796',0);
        INSERT INTO EQUIPES (nome,nome_b1,nome_b2,nome_b3,status) VALUES 
     ('BETA2','MT_11606','13TRF','E08115',1);
        INSERT INTO EQUIPES (nome,nome_b1,nome_b2,nome_b3,status) VALUES 
     ('BETA1','MT_07901','13TRF','E09516',1);
 
    
CURSOR cSTATUS IS 
        SELECT STATUS
        FROM EQUIPES
        WHERE STATUS = xSTATUS;        
 BEGIN 
    xSTATUS := STATUS;
    OPEN cSTATUS
    LOOP
     FETCH cSTATUS INTO xSTATUS
     EXIT WHEN cSTATUS%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE (xSTATUS);
    END LOOP;
    CLOSE cSTATUS;
  end;

create FUNCTION TAREFA (
 @nome VARCHAR (20),
 @area VARCHAR (30),
 )
 RETURNS @CABECALHO_TAREFA TABLE (
  oid INTEGER PRIMARY KEY AUTOINCREMENT, 
  nome VARCHAR (20),
  data_criacao date,
  area varchar (30),
  equipe_responsável int
   )
 as 
 begin 
       IF (@nome = NOME)        
          IF (xSTATUS = 1)          
               INSERT INTO @CABECALHO_TAREFA 
               SELECT NOME, 
                      GETDATE, 
                      NOME_B1'/'NOME_ B2'/'NOME_B3, 
                      STATUS 
               FROM EQUIPES
               WHERE NOME = @NOME
               RETURN (0)
               INSERT INTO LOG_PROCESS (codigo,descricao) VALUES (0,'SIM EXISTE NOME DE EQUIPE E ATIVO'||NOME)         
        ELSE IF (@nome <> NOME)
                  RETURN (-1)
               INSERT INTO LOG_PROCESS (codigo,descricao) VALUES (-1,'NAO EXISTE NOME DE EQUIPE')  
            ELSE IF (xSTATUS = 0)
                  RETURN (-2) 
               INSERT INTO LOG_PROCESS (codigo,descricao) VALUES (-2,'SI EXISTE NOME DE EQUIPE E INATIVO'||NOME)
            END if;
        end if;
   end;
   
 SELECT dbo.@CABECALHO_TAREFA (@NOME,@AREA);
END om_pkg_task; 
