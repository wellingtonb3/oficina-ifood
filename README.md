# Oficina-Ifood

## Projeto Final 2 do Curso Ciência de Dados by DIO - IFood


- [Desafio Parte 1: Criando o Diagrama no MysSQL Workbench](#desafio-parte-1---diagrama-do-banco-de-dados)


- [Desafio Parte 2: Criando o Script das Tabelas e Constraints](#desafio-parte-2---script-das-tabelas-e-constraints)


- [Desafio Parte 3: Inserindo Dados no Banco via MySQL](#desafio-parte-3---inserindo-dados-no-banco-via-mysql)


- [Desafio Parte 4: Criando Queries](#desafio-parte-4---criando-queries)


## Desafio Parte 1 - Diagrama do Banco de Dados
[DIAGRAMA DO BANCO DE DADOS]![image](https://github.com/wellingtonb3/oficina-ifood/blob/main/Oficina-Ifood.png)

[VOLTAR](#oficina-ifood)

## Desafio Parte 2 - Script das Tabelas e Constraints

### Criando o Banco de Dados

```mysql
-- CRIAÇÃO DO BANCO DE DADOS via MySQL Workbench

create database oficina;
use oficina;
```



### Criando a Tabela Cliente

```mysql
create table clients(
	idClient int auto_increment primary key,
        type_client ENUM('Pessoa Física', 'Pessoa Jurídica') not null,
        cpf char(11),
        fname varchar(10),
        minit char(3),
        lname varchar(20),
        cnpj char(14),
        businessname varchar(255),
        address varchar(30),
        phone_number varchar(11) not null,
        constraint unique_cpf_client unique (cpf),
        constraint unique_cnpj_client unique (cnpj)
);

alter table clients auto_increment = 1;
desc clients;
```

------------------------------------------------------------------------

### Criando a Tabela Pedido

```mysql
create table orders(
	idOrder int auto_increment primary key,
	idOrderClient int not null,
	title_order varchar(45) not null,
        problem_type ENUM ('Software', 'Hardware') not null,
        orderdescription varchar(255) not null,
	priority ENUM ('Baixa','Média', 'Alta') default "Baixa",
	constraint fk_orders_client foreign key (idOrderClient) references clients(idClient)                
);

alter table orders auto_increment = 1;
desc orders;
```


--------------------------------------------------------------------------

### Criando a Tabela Responsável Técnico
```mysql
create table technical(
	idTechnical int auto_increment primary key,
	worker_name varchar (100),
        sector varchar (45), 
	registration varchar (45),
	functions varchar (45)
);

alter table technical auto_increment = 1;
desc technical;
```

-------------------------------------------------------------------------


### Criando a tabela Pedido Gerado
```mysql
create table send_to(
	idSendTo int auto_increment primary key,
        idTechnical int, 
	descriptions varchar(255) not null,
        constraint fk_send_technical foreign key (idTechnical) references technical(idTechnical)		
);

alter table send_to auto_increment = 1;
desc send_to;
```


-------------------------------------------------------------------------

### Criando a tabela Ordem de Serviço
```mysql

create table service_order(
	idServiceOrder int auto_increment primary key,
        idService int,
	descriptions varchar(255),
        warranty ENUM ('SIM','NÃO') default 'NÃO' not null,
	solution_date DATE,
	constraint fk_service_send foreign key (idService) references send_to(idSendTo)
);

alter table service_order auto_increment = 1;
desc service_order;
```


--------------------------------------------------------------------------

### Comandos úteis
```mysql
-- show tables;
-- show databases;
-- use information_schema;
-- show tables;
-- desc table_constraints;
-- desc referential_constraints;
-- select * from referential_constraints;
-- select * from referential_constraints where constraint_schema = 'oficina';
```

[VOLTAR](#oficina-ifood)


## Desafio Parte 3 - Inserindo Dados no Banco via MySQL


### Acessando o Banco de Dados
```mysql

use oficina;
show tables;
```


---------------------------------------------------------------------------


### Populando a tabela Clientes
```mysql

desc clients;

-- type_client ENUM('Pessoa Física', 'Pessoa Jurídica') not null, cpf char(11), fname varchar(10),
--  minit char(3), lname varchar(20), cnpj char(14), businessname varchar(255), address varchar(30),
--  phone_number varchar(11) not null

insert into clients (type_client, cpf, fname, minit, lname, cnpj, businessname, address, phone_number) values
	('Pessoa Física', '47364568712' ,'João', 'B', 'Laffas', null , null, 'Rua Dez 44, Limão - Guto', '23 65448977'),
        ('Pessoa Jurídica', null, null, null, null, 37487209000167, 'App Gomes', 'Av Olavo 66, Pandas - Jetuba', '61 76339677'),
        ('Pessoa Física', '34265487818','Mauro', 'C', 'Carrins', null , null, 'Rua Seis 98, Timbre - Goiás', '21 76486722'),
        ('Pessoa Jurídica', null, null, null, null, 94287390000145, 'Achados', 'Av Garcia 756, Bamba - Jirau', '31 56238799'),
        ('Pessoa Jurídica', null, null, null, null, 32567359000167, 'Casakinhos', 'Av Jupiter 55, Cieaca - Mutu', '51 78442977');

          
select * from clients;
```


---------------------------------------------------------------------------


### Populando a tabela Pedido
```mysql

desc orders;
-- idOrderClient int not null, title_order varchar(45) not null, problem_type ENUM ('Software', 'Hardware') not null,
-- orderdescription varchar(255) not null, priority ENUM ('Baixa','Média', 'Alta') default "Baixa"

insert into orders (idOrderClient, title_order, problem_type, orderdescription, priority) values
	(1, 'Computador apita', 'Hardware', 'Cliente está usando o equipamento e de repente ele começa a apitar. Problema aleatório', 'Baixa'),
        (2, 'Monitor oscila', 'Hardware', 'Monitor LCD oscila a imagem. Cliente tem mexido na tela para continuar usando', 'Média'),
        (3, 'Windows com vírus', 'Software', 'Usuário estava baixando fotos quando o mesmo começou a dar mensagem de vírus', null),
        (4, 'Computador Não liga', 'Hardware', 'Usuário diz ter chegado de manhã e não conseguido ligar o notebook', 'Alta'),
        (5, 'Tela do notebook quebrada', 'Hardware', 'Cliente consegue utilizar o equipamento somente no monitor externo', 'Baixa');


select * from orders;

```


---------------------------------------------------------------------------


### Populando a tabela Responsável Técnico
```mysql

desc technical;
-- worker_name varchar (100), sector varchar (45), registration varchar (45), functions varchar (45);
insert into technical (worker_name, sector, registration, functions) values
	('Edivaldo', 'Alfândega', 04533, 'Técnico Pleno' ),
        ('Murilo', 'Diretoria', 04588, 'Técnico Senior'),
        ('Jennifer', 'Gerência', 04500, 'Gerente Geral');

select * from technical;
```


------------------------------------------------------------------------


### Populando a tabela Pedido Gerado
```mysql

desc send_to;
-- idTechnical int, descriptions varchar(255) not null, 
insert into send_to(idTechnical, descriptions) values
	(2, 'Solicitada a retirada do equipamento. Como o defeito é intermitente, necessário testes'),
        (1, 'Solicitada a retirada do monitor para possível manutenção'),
        (3, 'Feito auxílio para a execução do backup pelo cliente. Windows terá que ser reinstalado'),
        (2, 'Solicitada a retirada em modo de urgência'),
        (1, 'Agendado a retirada para conserto do equipamento');
                         

select * from send_to;
```


---------------------------------------------------------------------------


### Populando a tabela Ordem de Serviço
```mysql

desc service_order;
-- descriptions varchar(255), warranty ENUM ('SIM','NÃO') default 'NÃO' not null, solution_date DATE,
insert into service_order(idService, descriptions, warranty, solution_date) values
	(1,'Feito a troca do teclado que apresentava problemas intermitentes. Testes ok! ', default, '2023-04-07'),
        (2,'Feito a troca do LCD que estava com defeito. Liberado! OK!', 'SIM', '2023-04-08'),
	(3,'Já finalizando a reinstalação do Sistema Operacional', default, null),
        (4,'Equipamento em laboratório. Até o momento, detectado somente problema na fonte externa' , 'SIM', null),
        (5,'Aguardando retirada pelo motoboy, que já esteve no local mas não localizou usuário', default, null);

select * from service_order;
```

			
---------------------------------------------------------------------------


[VOLTAR](#oficina-ifood)


## Desafio Parte 4 - Criando Queries


### Formulando perguntas e respondendo com as Queries


*SELECIONE OS PEDIDOS EFETUADOS COM OS DADOS DE SEUS DEVIDOS COMPRADORES*

```mysql
select * from clients inner join orders ON idClient = idOrderClient;
```
-- ou
```mysql
select * from clients as c, orders as o where (c.idClient = o.idOrderClient);
```
---------------------------------------------------------------------------

  
*SELECIONE TODOS OS CLIENTES SEPARANDO POR PESSOA FÍSICA E JURÍDICA*
```mysql
select * from clients order by type_client;
```
---------------------------------------------------------------------------

  
*SELECIONE O TELEFONE DO CLIENTE COM ID NUMERO 8*
```mysql
select phone_number FROM clients where idClient = 8;
```
---------------------------------------------------------------------------
*VERIFIQUE QUAL A QUANTIDADE DE PRODUTOS DISPONÍVEIS EM SP*
```mysql
select sum(quantity) from productstock where stocklocation = 'São Paulo';
```
---------------------------------------------------------------------------
*SELECIONE OS PRODUTOS DISPONÍVEIS, SUAS QUANTIDADES E LOCALIDADES*
```mysql
select p.pname, ps.stocklocation, quantity from product as p, productstock as ps where idProduct = idProdStock;
```
---------------------------------------------------------------------------
*SELECIONE OS PEDIDOS QUE FORAM PAGOS POR BOLETO BANCÁRIO*

```mysql
select *  from payment where bank_slipcode > 0;
```
---------------------------------------------------------------------------
*SELECIONE OS PEDIDOS QUE FORAM PAGOS POR CARTÃO DE CRÉDITO*
```mysql
select *  from payment where card_number > 0;
```
---------------------------------------------------------------------------
*SELECIONE OS PEDIDOS QUE ESTÃO EM PREPARAÇÃO*
```mysql
select * from orders where delivery = "Em preparação";
```
---------------------------------------------------------------------------
### Comandos úteis para verificação das tabelas
```mysql
show tables;
select * from clients;
select * from orders;
select * from payment;
select * from product;
select * from product_seller;
select * from productorder;
select * from productstock;
select * from productsupplier;
select * from seller;
select * from stocklocation;
select * from supplier;
```


[VOLTAR](#oficina-ifood)





