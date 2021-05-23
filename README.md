# sql
create schema projeto1;
use projeto1;
Create table Country (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Name nvarchar(255)
);

Create table State (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Name nvarchar(255),
    CountryId int,
    FOREIGN KEY (CountryId) REFERENCES Country(Id)
);

Create table City (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Name nvarchar(255),
	StateId int,
    FOREIGN KEY (StateId) REFERENCES State(Id)
);

Create table Address (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	CityId int NOT NULL,
    foreign key (CityId) REFERENCES City(Id),
	Cep nvarchar(255) NOT NULL,
	Complement nvarchar(255) NULL
);
Create table User (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Name nvarchar(255) NOT NULL,
	BirthDate Date NULL,
	Email nvarchar(255) NOT NULL,
	AddressId int,
    FOREIGN KEY (AddressId) REFERENCES Address(Id)	
);
Create table Author (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Nome nvarchar(255)
);

Create table Publisher (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Name nvarchar(255),
	AddressId int,
    foreign key (AddressId) REFERENCES Address(Id)
);
Create table Book (
	Id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Name nvarchar(500),
	AuthorId int,
    FOREIGN KEY (AuthorId) REFERENCES Author(Id),
	PublicationDate date,
    Price float,
    PublisherId int,
    foreign key (PublisherId) REFERENCES Publisher(Id)
);

select * from Country;

insert into Country (Name) values ("Brasil");

select * from State;

insert into State (Name, CountryId) values ("São Paulo", 1);
insert into State (Name, CountryId) values ("Rio de Janeiro", 1);

select * from City;

insert into City (Name, StateId) values ("Paraty", 2);
insert into City (Name, StateId) values ("Sumaré", 1);
insert into City (Name, StateId) values ("Hortolandia", 1);

select * from Address;

insert into Address (CityId, Cep, Complement) values (1, 23900000, "Casa");
insert into Address (CityId, Cep) values (2, 12020363);
insert into Address (CityId, Cep, Complement) values (3, 13051567, "Casa");
insert into Address (CityId, Cep) values (3, 13051567);

select * from User;

insert into User (Name, BirthDate, Email, AddressId) values ("Keren", "2002-02-16", "keren@gmail.com", 3);
insert into User (Name, BirthDate, Email, AddressId) values ("Yugo", "1997-07-24", "yugo@gmail.com", 1);
insert into User (Name, BirthDate, Email, AddressId) values ("José", "1980-10-02", "jose@gmail.com", 2);

select * from Author;

insert into Author (Nome) values ("Lygia Fagundes");
insert into Author (Nome) values ("Kuro Gane");
insert into Author (Nome) values ("Stephenie Meyer");
insert into Author (Nome) values ("Rodrigo Silva");
insert into Author (Nome) values ("Seth Stephens-Davidowitz");

select * from Publisher;

insert into Publisher (Name, AddressId) values ("Abril", 2);
insert into Publisher (Name, AddressId) values ("Alta Books", 4);

select * from Book;

insert into Book (Name, AuthorId, PublicationDate, PublisherId, Price) values ("Casa Amarela", 1, "2000-01-10", 1, 15.99);
insert into Book (Name, AuthorId, PublicationDate, PublisherId, Price) values ("Over Lord", 2, "2010-06-02", 1, 45.00);
insert into Book (Name, AuthorId, PublicationDate, PublisherId, Price) values ("O Cetisismo da Fé", 1, "2000-01-10", 1, 51.00);
insert into Book (Name, AuthorId, PublicationDate, PublisherId, Price) values ("O Cetisismo da Fé", 1, "2000-01-10", 1, 28.99);
insert into Book (Name, AuthorId, PublicationDate, PublisherId, Price) values ("Todo mundo mente", 5, "2018-02-03", 1, 40.00);
insert into Book (Name, AuthorId, PublicationDate, PublisherId, Price) values ("As meninas", 1, "1973-02-03", 2, 32.00);

update Book set name = "O Ceticismo da Fé" where Id = 3;
update Book set name = "Crepúsculo", AuthorId = 3, PublicationDate = "2008-08-16" where Id = 4;
update Book set PublisherId = 2 where Id = 5;

delete from Book where Id = 3;
/* não teve como recuperar! */
select * from Book;
select * from Book where Name like "C%";

-- count -> conta a quantidade de ids da tabela book
select COUNT(Id) from Book;
-- avg -> retorna a média dos valores da Coluna Price na Tabela Book
select avg(Price) from Book;
-- sum -> retorna a soma dos valores da Coluna Price na Tabela Book
select sum(Price) from Book;
-- between -> seria basicamente o "entre" em português, um valor entre dois valores
select * from Book where Price Between 15 and 40;
select * from Book where AuthorId = 1 or AuthorId = 3;
select * from Book where not AuthorId = 1;
-- order by -> ordena na ordem ascendente(ASC) ou decrescente (DESC)
select Name, Price from Book order by Name ASC;
-- group by -> agrupa valores de uma coluna
select count(AuthorId), AuthorId from Book group by AuthorId;
-- join -> junta duas tabelas, sendo usados os comandos inner, left, right e entre outros
select a.nome as 'Author', b.name as 'TituloLivro'from Author a inner join Book b on (a.Id = b.AuthorId);
select a.nome as 'Author', b.name as 'TituloLivro'from Author a left join Book b on (a.Id = b.AuthorId);
select p.name, b.name as 'TituloLivro'from Publisher p right join Book b on (p.Id = b.PublisherId);

