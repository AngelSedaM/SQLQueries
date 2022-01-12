# SQLQueries
Almacenamiento de Queries 

Asignar un valor a una variable donde el From contenga mas variables
Ejemplo: SELECT colum FROM @var1.@var2.tabla
Esto puede servir cuando se utiliza un sp en distintos ambientes, (DEV, QA, PROD) donde en cada ambiente se tenga diferentes nombres a las bases de datos

DECLARE @Countrysql NVarchar (200)
DECLARE @CountryCode Varchar (10)
DECLARE @CountryparamDefinition    NVARCHAR(10)
DECLARE @tempCountryTable Table (TempVALUE NVARCHAR(100))
DECLARE @JDEDB              NVARCHAR(30)
DECLARE @JDEDBOwner         NVARCHAR(30)
DECLARE @Company			NVARCHAR(5)
SET @Company ='00409'
SET @JDEDB ='JDE_DV4R1'
SET @JDEDBOwner ='DV4R1DTA'


SET @Countrysql =(N'
SELECT MCRP02 
FROM ' + @JDEDB + '.' + @JDEDBOwner + '.F0006
WHERE LTRIM(RTRIM(MCMCU)) = REPLACE(LTRIM(REPLACE(REPLACE('+@Company+',''.'', ''''), ''0'', '' '')),'' '', ''0'')')
SET @CountryparamDefinition = N''
INSERT INTO @tempCountryTable  EXEC sp_executesql @Countrysql, @CountryparamDefinition
SET @CountryCode = (SELECT * FROM @tempCountryTable)
