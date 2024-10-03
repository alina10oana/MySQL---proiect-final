# MySQL - Proiect final - baza de date pentru o agenție imobiliară 

Scopul acestui proiect este de a prezenta cunoștiințele acumulate pe parcursul cursului de Testare Manuală. 
Astfel, am ales să creez o bază de date pentru o agenție imobiliară care își desfășoară activitatea în județul Cluj. Activitatea presupune închirierea sau vânzarea unor spații imobiliare din cadrul agenției imobiliare.

Structura este următoarea:

## Structura

![image](https://github.com/user-attachments/assets/541fd03c-3214-4f91-bbd7-43c997f65c2e)


### Tabele

#### 1. Clienti
	- "id_client" (int primary key auto_increment),
	- "denumire_client" (varchar (20)),
	- adresa varchar (70),
	- telefon int,
	- cod_fiscal int );


## Database Queries

### DDL (Data Definition Language)
Am folosit instrucțiuni pentru creare ștergere și modificare bază de date precum și tabelele din cadrul bazei de date. 
```sql
#creare baza de date cu denumirea Magazin 
	create database Magazin;

#stergere baza de date Magazin
	drop database Magazin; 

#creare baza de date cu denumirea Imobiliare2, destinata unei agentii imobiliare 
	create database Imobiliare2;

#creare tabela Clienti cu informatii privind clientii societatii
	create table Clienti (
		id_client int primary key auto_increment,
		denumire_client varchar (20),
		adresa varchar (70),
		telefon int,
		cod_fiscal int );
    
#creare tabela Facturi cu informatii privind facturi    
	create table Facturi (
		id_factura int primary key auto_increment,
		nr_factura int,
		serie_factura varchar (5),
		valoare int,
		cod_fiscal int);

 #creare tabela Contracte cu informatii privind contractele incheiate in cadrul societatii   
	 create table Contracte (
		id_contract int primary key auto_increment,
		nr_contract int,
		data_contract date ,
		cod_fiscal int );

#creare tabela Imobile cu informatii privind imobilele existente si supuse ofertei de vanzare/inchiriere a societatii   
	 create table Imobile (
		id_imobil int primary key auto_increment,
		nr_contract int,
		tip_imobil varchar (50) ,
		suprafata_imobil int,
		localitate_spatiu varchar (30),
		adresa_imobil varchar (70), 
		foreign key (nr_contract) references Contracte (id_contract));

#creare tabela Vanzari cu informatii privind imobilele vandute   
	 create table Vanzari (
		id_vanzare int primary key auto_increment, 
		id_imobil int,
		nr_contract int,
		pret int,
		data_vanzare date,
		foreign key (id_imobil) references Imobile (id_imobil));  

 #modificarea tabelei Facturi prin adaugarea coloanei id_client si setarea acesteia ca si cheie secundara
	 alter table Facturi 
	 add column id_client int
	 alter table Facturi
	 add foreign key (id_client) references Clienti (id_client);
        
        
 #modificarea tabelei Contracte prin adaugarea coloanei id_client
	  alter table Contracte
	  add column id_client int;
      
 #modificarea tabelei Contracte prin setarea coloanei id_client ca si cheie secundara    
	  alter table Contracte
	  add foreign key (id_client) references Clienti (id_client);
        

                
	- corelare tabela vanzari cu tabela contracte prin adaugarea unei noi chei secundare 
        alter table vanzari
        add column id_contract int,
        add foreign key (id_contract) references contracte (id_contract);
    
    - modificare tabela Contracte prin adaugarea coloanei denumire_banca
		alter table Contracte 
		add column denumire_banca varchar (40);
    
    - modificare tabela Contracte prin reordonarea coloanelor denumire_banca si cod_fiscal
		alter table Contracte 
		modify	column denumire_banca varchar (40) after data_contract;
    - stergere coloana pret din tabela Vanzari
		alter table Vanzari
		drop column pret;
    - modificare tabela Imobile prin stergerea coloanei adresa_imobil
		alter table Imobile
		drop column adresa_imobil;
```

### DML (Data Manipulation Language)	
În scopul utilizării bazei de date, folosind instrucțiuni DML, am populat tabelele cu diverse date pe care le-am considerat necesare.
De asemenea, am modificat informațiile prin actualizare și ștergere. 

```sql
    - populare cu informatii tabela Clienti
		insert into Clienti (denumire_client, adresa, telefon, cod_fiscal)
		values ('Popescu Mihai', 'localitate Cluj, strada Eminescu, nr.12', 0747474747, 187122),
		('SC ABC SRL', 'localitate Dej, piata 1 Mai, nr 12', 0364678909, 985621),
		('BIOSTAR SRL', 'localitate Dej, strada Bobului, nr.1, ap.2', 0758200321, 647832),
		 ('Popescu Mihai', 'localitate Cluj-Napoca, calea Baciului, nr.42', 0789546201, 1035269);
     
   - populare cu informatii tabela Facturi
		insert into Facturi (nr_factura, serie_factura, valoare, cod_fiscal, id_client)
		values (105, 'KLM', 50000, 187122, 1),
		(654, 'ABAC', 1500, 985621, 2),
		(2016, 'KVJ', 250000, 647832, 3),
		(80, 'SOET', 15000, 1035269, 4);
      
     - populare cu informatii tabela Contracte
		insert into Contracte (nr_contract, data_contract, denumire_banca, cod_fiscal, id_client)
		values (25, '2024-01-07', 'BT', 187122, 1),
		(35, '2023-10-27', 'Raiffeissen Bank', 985621, 2),
		(10, '2020-10-27', 'Raiffeissen Bank', 985621, 2),
		(39, '2023-12-02', 'BCR', 647832, 3),
		(101, '2022-05-06', 'BRD Societe General', 1035269, 1),
		(52, '2023-04-10', 'OTP BANK', 985621, 2);

      - populare cu informatii tabela Imobile
		insert into Imobile (nr_contract, tip_imobil, suprafata_imobil,localitate_spatiu)
		values (1, 'casa', 100, 'Dej'),
		(2, 'garsoniera', 25, 'Cluj-Napoca'),
		(3, 'garsoniera', 29, 'Apahida'),
		(6, 'apartament', 60, 'Cluj-Napoca'),
		(5, 'casa', 90, 'Dej'),
		(4, 'spatiu', 350, 'Cluj-Napoca'),
		(4, 'spatiu', 240, 'Cluj-Napoca'),
		(4, 'spatiu', 500, 'Cluj-Napoca');

       - populare cu informatii tabela Vanzari
		insert into Vanzari (id_imobil, nr_contract, data_vanzare)
		values (9, 1, '2024-01-31'),
		(10, 2, '2023-10-30'),
		(11, 3, '2020-11-01'),
		(12, 6, '2023-05-30'),
		(13, 5, '2022-05-30'),
		(14, 4, '2023-12-29'),
		(15, 4, '2023-12-28'),
		(16, 4, '2023-12-20');

    - actualizare coloana suprafata_imobil prin cresterea cu 10 a suprafetei
		update Imobile set suprafata_imobil=suprafata_imobil+10;

    - stergere informatii din tabela Facturi aferent clientului cu id 4
		delete from Facturi where id_factura=(4);  
    
```

  ### DQL (Data Query Language)
    
    Vizualizarea informațiilor din cadrul tabelelor 
   ```sql
    - interogare simplă pe toată tabela Facturi;
		select * from Facturi;
    - interogare simpla pe toata tabela Imobile;
		select * from Imobile;
     - interogare simplă pe toată tabela Clienti
		select * from Clienti;
     - interogare simplă pe toată tabela Contracte;
		select * from Contracte;

    - returnarea valorilor facturilor pe coloana id_client
		select valoare, id_client from	Facturi;
    - returnarea valorilor pentru clientul cu id=1
		select valoare from Facturi where id_client=1;
    - returnarea clientilor doar pentru cei care au contractul incepand cu anul 2023 la banca BCR
		select * from Contracte where denumire_banca='BCR' and data_contract like '2023%';

   
    
   
    
    - interogare simpla pe toata tabela Vanzari;
		select * from Vanzari;
    
    - stergerea inregistrarii aferenta datei de vanzare 2023-12-28 din tabela Vanzari
		delete from Vanzari where id_vanzare=7;
    
    - returnarea clientilor cu contracte ;
		select * from Clienti cross join	contracte;
    
    - returnarea clientilor cu contracte, ordonati dupa denumire banca;
		select * from Clienti cross join	contracte order by denumire_banca;
    
    - returnarea clientilor cu datele din contract;
		select * from Clienti inner join Contracte on clienti.id_client=contracte.id_client;
    
    - returnarea denumirii si adresei clientilor precum si nr de contract aferent si ordonate dupa nr contractului;
		select denumire_client, adresa, nr_contract from Clienti inner join Contracte on clienti.id_client=contracte.id_client order by nr_contract;
    
    - returnarea informatiilor privind denumirea si codul fiscal al clientilor ordonate dupa codul fiscal;
		select denumire_client, facturi.cod_fiscal from Clienti right join Facturi on clienti.id_client=facturi.id_client order by facturi.cod_fiscal;
    
	- returnarea imobilelor vandute in cadrul agentiei imobiliare si ordonate dupa suprafata imobilului;
		select * from imobile left join vanzari on imobile.id_imobil=vanzari.id_imobil order by suprafata_imobil;
    
    - returnarea celei mai mici valori a unui imobil;
		select min(valoare) from Facturi;
    
    - returnarea celei mai mari valori a unui imobil;
		select max(valoare) from Facturi;
        
    - returnarea tuturor contractelor inregistrate;
		select count(*) from contracte;
    
    - returnarea valorii medie a imobilelor aferente clientului 2;
		select avg (valoare) from Facturi where id_client=2;
    
    - returnarea valorii vanzarilor totale;
		select sum(valoare) from facturi;
      
	- returnarea suprafetei maxime din cadrul imobilelor existente in oferta agentiei imobiliare;
		select max(suprafata_imobil) from Imobile;
      
	- returnarea informatiilor prvind suprafata minima si maxima a imobilelor din oferta agentiei imobiliare;
      select 
		min(suprafata_imobil) as "Suprafata minima",
		max(suprafata_imobil) as "Suprafata maxima"
		from Imobile;
        
	- returnarea informatiilor privind valoarea medie a imobilelor din oferta ordonate dupa id_client si este peste valoarea 3000;
		select id_client,
		avg(valoare) as "Valoare medie a imobilelor din oferta"
		from facturi group by id_client
		having avg(valoare)>3000;
        
	- returnarea datei minime si maxime la care au fost semnate contractele cu clientii, dupa anul 2020;
		select
			min(data_contract) as "Data minima",
            max(data_contract) as "Data maxima" 
            from Contracte having min(data_contract) like '202%';
        
    - returnarea imobilelor cu valoare peste valoarea medie a imobilelor din oferta agentiei; 
		select nr_factura, serie_factura, valoare from facturi
			where valoare > (select avg(valoare) from facturi);
