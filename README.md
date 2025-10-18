# DIO.Randstad-Analise_de_Dados.Construindo-seu-Primeiro-Projeto-L-gico-de-Banco-de-Dados.E-COMMERCE
Queries + PNG do E-COMMERCE, usado o conhecimento da aula, apenas foi usado o uso de IA para compreendimento de assuntos e para o teste de insert, delete e update, feito no curso da DIO: Randstad-Analise_de_Dados.
Modelo:
<img width="857" height="642" alt="E-COMMERCE" src="https://github.com/user-attachments/assets/fe037f3f-61ef-4007-b237-4a93d9346fdf" />
Query:
create database ecommerce;
use ecommerce;
create table Cliente(
idCliente INT auto_increment primary key not null,
Pnome VARCHAR(45) not null,
Nmeioinicial varchar(45),
PF_PJ ENUM('PJ','PF') not null,
CPF_CNPJ varchar(45) not null unique,
Endereco varchar(45),
Datadenascimento DATE not null
);
create table Pedido(
idPedido INT auto_increment primary key,
Status_ ENUM('Em andamento', 'Em processamento', 'Concluido') default 'Em processamento',
Descricao varchar(45),
Frete double,
idCliente INT,
foreign key (idCliente) references Cliente(idCliente)
);
create table Produto(
idProduto INT auto_increment primary key not null,
Categoria enum('Eletronico','Vestimenta','Brinquedo','Alimento','Moveis'),
Descricao VARCHAR(45),
Valor DOUBLE
);
create table Pagamento(
idPagamento INT auto_increment primary key not null,
Tipo ENUM('Debito', 'Credito', 'Boleto', 'Transferencia Bancaria', 'PIX'),
Detalhes VARCHAR(45)
);
create table Entrega(
idEntrega INT auto_increment primary key not null,
Status_ ENUM('Em transporte', 'Entregue', 'Indisponivel', 'Remetente ausente'),
Codigoderastrieio VARCHAR(45)
);
create table Pagamento_Forma(
idPagamento_Forma INT auto_increment primary key not null,
Valor DOUBLE
);
create table Estoque(
idEstoque INT auto_increment primary key not null,
Local varchar(45)
);
create table Fornecedor(
idFornecedor INT auto_increment primary key not null,
RazaoSocial varchar(45),
CNPJ varchar(45)
);
create table Terceiro(
idTerceiro INT auto_increment primary key not null,
RazaoSocial varchar(45),
Local varchar(45),
Nomefantasia varchar(45)
);
create table Terceiro_Produto(
Terceiro_idTerceiro INT,
Produto_idProduto INT,
foreign key (Terceiro_idTerceiro) references Terceiro(idTerceiro),
foreign key (Produto_idProduto) references Produto(idProduto)
);
create table Fornecedor_Produto(
Forncedor_idFornecedor INT,
Produto_idProduto INT,
foreign key (Forncedor_idFornecedor) references Fornecedor(idFornecedor),
foreign key (Produto_idProduto) references Produto(idProduto)
);
create table Estoque_Produto(
Quantidade INT,
Estoque_idEstoque INT,
Produto_idProduto INT,
foreign key (Estoque_idEstoque) references Estoque(idEstoque),
foreign key (Produto_idProduto) references Produto(idProduto)
);
create table Pagamento_Forma_Pagamento(
Pagamento_Forma_idPagamento_Forma INT,
Pagamento_idPagamento INT,
foreign key (Pagamento_Forma_idPagamento_Forma) references Pagamento_Forma(idPagamento_Forma),
foreign key (Pagamento_idPagamento) references Pagamento(idPagamento)
);
-- ======================
-- TESTES DE INSERT
-- ======================
insert into Cliente (Pnome, Nmeioinicial, PF_PJ, CPF_CNPJ, Endereco, Datadenascimento)
values ('Joao', 'A', 'PF', '12345678900', 'Rua A, 10', '2000-05-10');

insert into Pedido (Status_, Descricao, Frete, idCliente)
values ('Em andamento', 'Pedido de teste', 25.90, 1);

insert into Produto (Categoria, Descricao, Valor)
values ('Eletronico', 'Fone Bluetooth', 199.90);

insert into Pagamento (Tipo, Detalhes)
values ('PIX', 'Pagamento instantâneo');

insert into Entrega (Status_, Codigoderastrieio)
values ('Em transporte', 'BR123456789');

insert into Pagamento_Forma (Valor)
values (199.90);

insert into Estoque (Local)
values ('Centro de Distribuição 1');

insert into Fornecedor (RazaoSocial, CNPJ)
values ('Tech Imports LTDA', '11222333444455');

insert into Terceiro (RazaoSocial, Local, Nomefantasia)
values ('LogExpress', 'São Paulo', 'LogEx');

insert into Terceiro_Produto (Terceiro_idTerceiro, Produto_idProduto)
values (1, 1);

insert into Fornecedor_Produto (Forncedor_idFornecedor, Produto_idProduto)
values (1, 1);

insert into Estoque_Produto (Quantidade, Estoque_idEstoque, Produto_idProduto)
values (50, 1, 1);

insert into Pagamento_Forma_Pagamento (Pagamento_Forma_idPagamento_Forma, Pagamento_idPagamento)
values (1, 1);

-- RECUPERAÇÕES SIMPLES COM SELECT

select * from Cliente;
select * from Pedido;
select * from Produto;
select * from Pagamento;
select * from Entrega;
select * from Estoque;
select * from Fornecedor;
select * from Terceiro;

-- FILTROS COM WHERE

select * from Produto where Valor > 100;
select * from Pedido where Status_ = 'Em andamento';
select * from Cliente where Pnome like 'J%';

-- EXPRESSÕES DERIVADAS

select 
    Pnome, 
    year(curdate()) - year(Datadenascimento) as Idade,
    concat(Pnome, ' ', Nmeioinicial) as NomeCompleto
from Cliente;

select 
    Descricao, 
    Valor, 
    Valor * 0.9 as Valor_com_Desconto
from Produto;

-- ORDER BY

select * from Produto order by Valor desc;
select * from Cliente order by Datadenascimento asc;

-- HAVING (condições sobre grupos)

select 
    Categoria, 
    avg(Valor) as MediaPreco
from Produto
group by Categoria
having avg(Valor) > 100;

-- JOIN ENTRE TABELAS

select 
    c.Pnome, 
    p.Descricao as Pedido, 
    pr.Descricao as Produto, 
    pr.Valor
from Cliente c
join Pedido p on c.idCliente = p.idCliente
join Produto pr on pr.idProduto = 1;

-- Exemplo com múltiplas junções
select 
    c.Pnome,
    p.Descricao as Pedido,
    pa.Tipo as TipoPagamento,
    pr.Descricao as Produto,
    e.Status_ as StatusEntrega
from Cliente c
join Pedido p on p.idCliente = c.idCliente
join Pagamento pa on pa.idPagamento = 1
join Produto pr on pr.idProduto = 1
join Entrega e on e.idEntrega = 1;

-- ======================
-- TESTES DE UPDATE
-- ======================
update Cliente set Endereco = 'Rua Nova, 123' where idCliente = 1;
update Pedido set Status_ = 'Concluido' where idPedido = 1;
update Produto set Valor = 179.90 where idProduto = 1;
update Estoque_Produto set Quantidade = 45 where Produto_idProduto = 1;

-- Verificar UPDATE
select * from Cliente where idCliente = 1;
select * from Pedido where idPedido = 1;
select * from Produto where idProduto = 1;
select * from Estoque_Produto where Produto_idProduto = 1;
-- ======================
-- TESTES DE DELETE
-- ======================

delete from Terceiro_Produto where Terceiro_idTerceiro = 1;
delete from Terceiro where idTerceiro = 1;

delete from Fornecedor_Produto where Forncedor_idFornecedor = 1;
delete from Fornecedor where idFornecedor = 1;

delete from Pedido where idPedido = 1;
delete from Cliente where idCliente = 1;

-- Verificar DELETE
select * from Terceiro where idTerceiro = 1;
select * from Terceiro_Produto where Terceiro_idTerceiro = 1;
select * from Fornecedor where idFornecedor = 1;
select * from Fornecedor_Produto where Forncedor_idFornecedor = 1;
select * from Pedido where idPedido = 1;
select * from Cliente where idCliente = 1;

-- drop da database
drop database ecommerce;
