## SQL: stored procedure downstream processing

A limitation of SQL stored procedures is the awkwardness of doing any more SQL
work with the results. There's an excellent summary of the possible methods
[here](http://www.sommarskog.se/share_data.html). A table valued function is
often a great solution, but I have run into performance issues where a
multi-statement function needs to insert results into an output table. There is
also the fact that working with TVF's adds an extra degree of complexity to any
setup - I have worked in an inexperienced team environment where the differences
between TVF's and procedures would be enough to significantly disrupt the
learning process (e.g. the need to define an output table and the differences in
passing parameters, especially lists, from SSRS)

The `INSERT-EXEC` method is a good alternative to TVF's, but this will introduce
[connascence](https://en.wikipedia.org/wiki/Connascence) between the source
procedure and any downstream queries - if columns are added/removed in the
procedure, a downstream `INSERT-EXEC` will have an out of date column definition
and will not run. I wanted a method whereby the source procedure could be worked
on without the user having to go on a subsequent hunt for dependent queries to
make sure they all still work. This is a key consideration since the vast
majority of developer work is spent maintaining/modifying code rather than
writing it - making changes is the task that should be made easy, even if this
involves setting up some extra architecture initially

The function I came up with served this purpose and also transferred almost all
the complexity into the function itself or into the downstream queries. Keeping
the source procedure itself simple is important since it is being used in
multiple places and is therefore more likely to have future changes made. And
even though some source procedures - those using temporary tables - would need
to include an output parameter and a `NoResults` parameter, this need only be
set up once and would need no subsequent attention when making changes (bar a
complete restructure of the procedure)

---

```sql
CREATE FUNCTION GenerateProcResultsTable
(
@ProcDatabase nvarchar(128)
, @ProcSchema nvarchar(128)
, @ProcName nvarchar(128)
, @TempTableName nvarchar(128)
, @TempColumnName nvarchar(128)
, @ParamString nvarchar(max) = ' '
, @OptionalXML xml = NULL
)
RETURNS nvarchar(max)
AS
BEGIN

/*
function that crates a statement to create and populate a temporary table with stored procedure results
using this means changes can be made to the original procedure without upsetting downstream queries
the statement is actually executed in the caller query
, which means that a local temp table can be used providing it is created first with a placeholder column
*/

/*
note that procedures including temp tables / dynamic sql canot return column lists using sys.dm_exec_describe_first_result_set

in these circumstances, the procedure can be set to output an appropriate structure via an xml output parameter called @StructureXML
SET @StructureXML =
(SELECT [name], [system_type_name] FROM sys.dm_exec_describe_first_result_set ('SELECT * FROM #ProcData',NULL,NULL) FOR XML AUTO)

it would also be necessary to prevent the procedure from returning any results when fetching this XML
IF @NoResults = 1
RETURN

the table structure can then be passed into the function using the @OptionalXML parameter
DECLARE @OutputXML xml
EXEC Schema.Proc @StructureXML = @OutputXML OUTPUT, @NoResults = 1
PRINT dbo.ufn_GenerateProcResultsTable('DB','Schema','Proc','#Results','_',' ',@OutputXML)

*/

IF @ParamString IS NULL SET @ParamString = ' ';

DECLARE @StructureTbl table([name] nvarchar(128), [system_type_name] nvarchar(128));
DECLARE @SQLcmd nvarchar(max);

IF @OptionalXML IS NULL
-- fetch structure description based on the supplied procedure name
BEGIN
INSERT INTO @StructureTbl ([name], [system_type_name])
SELECT [name], [system_type_name]
FROM sys.dm_exec_describe_first_result_set ('EXEC ' + @ProcDatabase + '.' + @ProcSchema + '.' + @ProcName + @ParamString,NULL,NULL)
END

ELSE
BEGIN
-- use a structure description supplied as a function parameter
INSERT INTO @StructureTbl ([name], [system_type_name])
SELECT
Tab.Col.value('@name','nvarchar(128)') AS [name]
,Tab.Col.value('@system_type_name','nvarchar(128)') as [system_type_name]
FROM
@OptionalXML.nodes('sys.dm_exec_describe_first_result_set') Tab(Col)
END
;

IF (SELECT COUNT([name]) FROM @StructureTbl) = 0
BEGIN
SET @SQLcmd = '
-- No columns returned from target procedure
-- if procedure involves temp tables / dynamic sql, read the comments in GenerateProcResultsTable for a possible fix
'
END

ELSE
BEGIN
-- string the column descriptions together into a single alter statement
-- remove the original placeholder column
-- insert procedure results into temporary table
SET @SQLcmd = 'ALTER TABLE ' + @TempTableName + ' ADD ' +
(SELECT STUFF ((SELECT ',[' + [name] + '] ' + [system_type_name] FROM @StructureTbl FOR XML PATH('')), 1, 1, '')) + ';
ALTER TABLE ' + @TempTableName + ' DROP COLUMN [' + @TempColumnName + '];
INSERT INTO ' + @TempTableName + ' EXEC ' + @ProcDatabase + '.' + @ProcSchema + '.' + @ProcName + @ParamString + ';'
END
;

RETURN @SQLcmd
END
```
