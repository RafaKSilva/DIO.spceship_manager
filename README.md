# Projeto de Banco de dado de um Gerenciado de Naves Espaciais

O projeto de exemplo para exercitar os conhecimentos da linguagem de SQL.

## Script de Criação do Banco de dados e das Tabelas

### Criação do Banco de dados

```sql
CREATE DATABASE SpaceshipManager
```

### Definir o Banco de dados a ser alterado

```sql
USE SpaceshipManager
```

### Criar tabela de Naves

```sql
CREATE TABLE Naves (
	IdNave int NOT NULL,
	Nome varchar(100) NOT NULL,
	Modelo varchar(150) NOT NULL,
	Passajeiros int NOT NULL,
	Carga float NOT NULL,
	Classe varchar(100) NOT NULL
)

ALTER TABLE Naves ADD CONSTRAINT FK_Naves 
                     PRIMARY KEY (IdNave) 
```

### Criar tabela de Planetas

```sql
CREATE TABLE Planetas (
	IdPlaneta int NOT NULL,
	Nome varchar(50) NOT NULL,
	Rotacao  float NOT NULL,
	Orbita float NOT NULL,
	Diametro float NOT NULL,
	Clima varchar(50) NOT NULL,
	Populacao int NOT NULL
)

ALTER TABLE Planetas ADD CONSTRAINT PK_Planetas 
                        PRIMARY KEY (IdPlaneta);

```

### Criar tabela de Pilotos

```sql
CREATE TABLE Pilotos (
	IdPiloto int NOT NULL,
	Nome varchar(200) NOT NULL,
	AnoNascimento varchar(10) NOT NULL,
	IdPlaneta int NOT NULL
)

ALTER TABLE Pilotos ADD CONSTRAINT PK_Pilotos 
                       PRIMARY KEY (IdPiloto)

ALTER TABLE Pilotos ADD CONSTRAINT FL_Planetas_Pilotos 
                       FOREIGN KEY (IdPlaneta) REFERENCES Planeta(IdPlaneta)
```

### Criar trabela de relação Piloto e Planeta


```sql
CREATE TABLE PilotosNaves (
	IdPiloto int NOT NULL,
	IdNave int NOT NULL,
	FlagAutorizado bit NOT NULL
)

ALTER TABLE PilotosNaves ADD CONSTRAINT PK_PilotosNaves 
                            PRIMARY KEY (IdPiloto,IdNave)

ALTER TABLE PilotosNaves ADD CONSTRAINT FK_PilotosNaves_Pilotos 
                            FOREIGN KEY (IdPiloto) REFERENCES Pilotos(IdPiloto)

ALTER TABLE PilotosNaves ADD CONSTRAINT FK_PilotosNaves_Naves 
                            FOREIGN KEY (IdNave) REFERENCES Naves(IdNave)

ALTER TABLE PilotosNaves ADD CONSTRAINT DF_PilotosNaves_FlagAutorizado 
                             DEFAULT(1) FOR FlagAutorizado
```

### Criar tabela de relação Piloto e Nave

```sql
CREATE TABLE HistoricoViagens (
	IdNave int NOT NULL,
	IdPiloto int NOT NULL,
	DtSaida datetime NOT NULL,
	DtChegada datetime NULL
)
ALTER TABLE HistoricoViagens ADD CONSTRAINT FK_HistoricoViagens_PilotoNaves 
                                FOREIGN KEY (IdPiloto,IdNave) REFERENCES PilotosNaves(IdPiloto,IdNave)

ALTER TABLE HistoricoViagens CHECK CONSTRAINT FK_HistoricoViagens_PilotoNaves
```