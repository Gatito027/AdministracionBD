# Creación de una Base de Datos

```sql
--Creacion de una base de datos
create database paquitabd
on primary
(
	Name = paquitabdData, filename = 'C:\datanueva\paquitabd.mdf',
	size = 50MB, --El tamaño minimo es de 512kb, el preterminado es de 1MB
	filegrowth = 25%, --el default es 10%
	maxsize = 400MB
)
log on
(
	name=paquitabdLog, filename = 'C:\lognueva\paquitabd_log.ldf',
	size = 25MB,
	filegrowth = 25%
)



--crear un archivo adicional
alter database paquitabd
add file
(
	name = 'PaquitaDataNDF',
	filename = 'C:\datanueva\paquitabd2.ndf',
	size = 25MB,
	maxsize = 500MB,
	filegrowth = 10MB --El minimo es de 64kb
)to filegroup[PRIMARY];



--creacion de un FileGroup adicional
alter database paquitabd
add filegroup SECUNDARIO
go

--creacion de un archivo asociado al FileGroup
alter database paquitabd
add file
(
	name = 'Paquitabd_parte1',
	filename = 'C:\datanueva\paquitabd_SECUNDARIO.ndf'
)to filegroup SECUNDARIO;






--Modificar el gupo primario
use master

alter database paquitabd
modify filegroup [SECUNDARIO] DEFAULT;

use paquitabd

create table comparadocontigo(
	id int not null identity(1,1),
	nombredelanimal nvarchar(100) not null,
	defectos nvarchar(max) not null,
	constraint pk_comparadocontigo
	primary key (id),
	constraint unico_nombre3
	unique(nombredelanimal)
);

--revision Del Estado de la opcion de ajuste automatico del tamaño de archivos
select DATABASEPROPERTYEX('paquitabd','ISAUTOSHRINK');

--cambia la opcion de autoShrink a true
ALTER DATABASE paquitabd 
set AUTO_SHRINK ON WITH NO_WAIT
go
```