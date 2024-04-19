# Estudo de caso da Casa olivera
## Desenvolvimento das modelagens do estudo de casa
### o estudo de caso
#### Casa Olivera

Casa Oliveira
Roberto é dono de um mercado no bairro de Vargem Grande, na cidade de 
Tupã. Ele herdou o negócio de seu pai, Gumercindo Oliveira, ela foi aberta em 
1978 na garagem da casa da família, era uma pequena quitanda. Com o passar 
dos anos o negócio cresceu e Gumercindo foi obrigado a ir para outro ponto 
maior e ali permaneceu até os dias atuais.
Roberto, que agora é o novo dono do mercado continuou o negócio seguindo 
da mesma forma que o pai. Ele comprava diretamente com os fornecedores 
grandes volumes de produtos e armazenava em seu estoque. As vezes ele 
comprava muitos produtos que ainda havia em estoque causando uma 
sobrecarga de produtos, ele também tinha muitos produtos estragados, tais 
como: frutas, legumes, iogurtes, leites, frango etc. Também havia muitos 
produtos com o prazo de validade vencido.
Os funcionários eram poucos e faziam muitas coisas ao mesmo tempo. O 
açougueiro também ajudava no estoque, a moça da limpeza ajudava na 
organização dos produtos das prateleiras, além de ajudar na padaria, quanto o 
caixa estava vazio o operador ajudava a repor os laticínios e a limpar a loja. O 
repositor também fazia operação no caixa.
Ao realizar a venda o Roberto, que sabia o nome de quase todos os clientes, 
anotava em um caderno todos os produtos que vendia e que havia em estoque. 
Ao fim do dia , Roberto pegava o caderno de fazia os cálculos de o quanto 
havia vendido, somando o faturamento e realizando a atualização do estoque. 
Isso é feito todos os dias e toma um tempo considerável para que tudo seja 
feito.
Roberto fechava a loja as 18h, mas só ia para casa as 22h, após fazer todas as 
operações necessárias. Mesmo assim o negócio vai bem e Roberto pretende ir 
para outro ponto e aumentar o volume de negócios e contratar novos 
funcionários.
Marica, esposa de Roberto, vem conversando com ele há muito tempo para que 
ele contrate uma empresa para construir um sistema de informática para 
gerenciar o negócio e reduzir o tempo que ele passa trabalhando e tenha maior 
organização dos produtos, maior lucratividade e melhorar a gestão.
Com a intenção de aumentar o negócio, Roberto está disposto a informatizar 
sua empresa. Vamos ajudá-lo. Iremos começar construindo o banco de dados

#### Entidades levantadas:

- Estoque;
- Produto;
- Venda;
- Detalhes da Venda;
- Cliente;
- Funcionário;
- Cargos;
- Usuário;
- Pagamento;
- Fornecedor;
- Endereço;
- Contato;

#### Primeiro diagrama

!["Primeiro Diagrama"](Diagrama.png)

#### Tabela Casa Oliveira

!["esquema Casa Olivera"](CAsaOliveira.png)

#### Modelo Físico: Codigo que gera o banco de dados
```sql


-- Comando para criar um novo bando de dados 

CREATE DATABASE casaoliveira;

-- Comando para selecionar banco de dados criado

USE casaoliveira;
 
-- Comando para criação de tabela

CREATE TABLE usuario(
idusuario int auto_increment primary key,
nomeusuario varchar(50) not null unique, 
senha varchar(255) not null,
criadoem datetime default current_timestamp(),
foto varchar(250) not null

);
 
CREATE TABLE funcionario(
idfuncionario int auto_increment primary key,
nomefuncionario varchar(50) not null,
cpf varchar(13) not null unique,
idcargo int not null,
expediente varchar(50) not null,
idusuario int not null,
idendereco int not null,
idcontato int not null
);
 
CREATE TABLE cargo(
idcargo int auto_increment primary key,
titulocargo varchar(30) not null unique,
salario varchar(7.2) not null,
departamento varchar(30) not null unique
);
 
CREATE TABLE contato(
idcontato int auto_increment primary key,
telefoneresdencial varchar(15),
telefonecelular varchar(15) not null unique,
email varchar(100) not null unique
);
 
CREATE TABLE endereco(
idendereco int auto_increment primary key,
tipologradouro enum("Rua","Avenida","Travessa","Estrada","Viela","Praça","Alameda"),
logradouro varchar(50) not null,
numero varchar(10) not null,
complemento varchar(30),
bairro varchar(30) not null,
cep varchar(10) not null,
cidade varchar(30) not null,
estado varchar(20) not null
);

CREATE TABLE fornecedor(
idfornecedor int auto_increment primary key,
razaosocial varchar(50) not null unique,
nomefantasia varchar(50) not null,
cnpj varchar(15) not null unique,
idcontato int not null,
idendereco int not null
);
 
CREATE TABLE produto(
idproduto int auto_increment primary key,
nomeproduto varchar(50) not null unique,
categoria enum("Frios","Limpeza","Laticinio","Cereais","Açougue","Bebidas","Pães","Hortfrute"),
descricao text not null,

idfornecedor int not null,

preco decimal(6,2) not null

);
 
CREATE TABLE lote(

idlote int auto_increment primary key,

datafabricacao date not null,

datavalidade date null,

idproduto int not null

);
 
CREATE TABLE estoque(

idestoque int auto_increment primary key,

idlote int not null,

quantidade int not null,

dataestoque datetime default current_timestamp()

);

CREATE TABLE cliente(

idcliente int auto_increment primary key,

nomecliente varchar (50) not null,

cpf varchar (13) not null unique,

datanascimento date not null,

idcontato int not null,

idendereco int not null

);
 
CREATE TABLE venda(

idvenda int auto_increment primary key,

idcliente int not null,

idfuncionario int not null,

dathora datetime default current_timestamp(),

total decimal(7,2) not null

);
 
CREATE TABLE detalhevenda(

iddetalhevenda int auto_increment primary key,

idvenda int not null,

idproduto int not null,

quantidade int not null,

subtotal decimal(7,2)

);
 
CREATE TABLE pagamento(

idpagamento int auto_increment primary key,

idvenda int not null,

formapagamento enum("Dinheiro","Crédito","Debito","Pix"),

observacao text not null

);

-- Alterar a tabela Funcionario para adicionar uma chave

-- estrangeira e um relacionamento com a tabela usuario
 
ALTER TABLE funcionario

ADD CONSTRAINT `fk.func_pk.usuario`

FOREIGN KEY funcionario(`idusuario`)

REFERENCES usuario(`idusuario`);
 
-- Relacionamento da tabela funcionario com cargo
 
ALTER TABLE funcionario

ADD CONSTRAINT `fk.func_pk.cargo`

FOREIGN KEY funcionario(`idcargo`)

REFERENCES cargo(`idcargo`);
 
ALTER TABLE funcionario

ADD CONSTRAINT `fk.func_pk.endereco`

FOREIGN KEY funcionario(`idendereco`)

REFERENCES endereco(`idendereco`);
 
ALTER TABLE funcionario

ADD CONSTRAINT `fk.func_pk.contato`

FOREIGN KEY funcionario(`idcontato`) 

REFERENCES contato(`idcontato`);
 
-- Adicionar uma constraint(restrição) a tabela fornecedor 

-- para adicionar uma chave estrangeira e estabelecer relacionamento

-- com as tabelas contatos
 
ALTER TABLE fornecedor 

ADD CONSTRAINT `fk.forne_pk.contato`

FOREIGN KEY fornecedor(`idcontato`) 

REFERENCES contato(`idcontato`);
 
ALTER TABLE fornecedor 

ADD CONSTRAINT `fk.forne_pk.endereco`

FOREIGN KEY fornecedor(`idendereco`) 

REFERENCES endereco(`idendereco`);
 
-- Altera a tabela lote adicionar adicionar uma chave estrageira 

-- e realizando o relacionamento com a tabela produto
 
ALTER TABLE lote 

ADD CONSTRAINT `fk.lote_pk.produto`

FOREIGN KEY lote(`idproduto`) 

REFERENCES produto(`idproduto`);
 
-- Adicionar constrait a tabela estoque
 
ALTER TABLE estoque 

ADD CONSTRAINT `fk.estoque_pk.lote`

FOREIGN KEY estoque(`idlote`) 

REFERENCES lote(`idlote`);
 
ALTER TABLE cliente

ADD CONSTRAINT `fk.cliente_pk.contato`

FOREIGN KEY cliente(`idcontato`) 

REFERENCES contato(`idcontato`);
 
ALTER TABLE cliente

ADD CONSTRAINT `fk.cliente_pk.endereco`

FOREIGN KEY cliente(`idendereco`) 

REFERENCES endereco(`idendereco`);
 
ALTER TABLE venda 

ADD CONSTRAINT `fk.venda_pk.cliente`

foreign key venda(`idcliente`)

references cliente(`idcliente`);

ALTER TABLE venda 

ADD CONSTRAINT `fk.venda_pk.func`

foreign key venda(`idfuncionario`)

references funcionario(`idfuncionario`);

ALTER TABLE detalhevenda

ADD CONSTRAINT `fk.detalhevenda_pk.venda`

foreign key detalhevenda(`idvenda`)

references produto(`idproduto`);

ALTER TABLE pagamento

ADD CONSTRAINT `fk.pagamento_pk.venda`

foreign key pagamento(`idvenda`) 

references venda(`idvenda`);
 
-- Comando que apaga o bandco de dados

-- drop database csaoliveira;


```

#### Modelo relacional do banco de dados 
!['modelo Relacional'](modelo_relacional_casaolivera.png)
