Just joined GitHub in order to upload a SQL Server script which is too large for Stack overflow. Stack overflow only allows 30,000 characters. My script contains 42,000. I hope that GitHub will allow me to enter all 43,000 characters.

This is a solution for SQL Server. The objective is to calculate the result of a numeric expression held in a character string.  	

The user scalar function 'mthscnvrt10' is called in the usual ways:

Ex1. select [myaksess].[dbo].[mthscnvrt10]('(((958577566/94447)+966988999)*785)')

Ex2. select top 10 pict, [myaksess].[dbo].[mthscnvrt10](pict) as mathpict_result from [myaksess].[dbo].[pick4s]


Unfortunately, the examples above are being truncated by the text editor window. However the functions are called in the usual ways.

Example1 above is using numeric string "(((958577566/94447)+966988999)*785)" as it's parameter.

Example2 above is using numeric string "column pict from table [myaksess].[dbo].[pick4s]" as it's parameter.


If SQLServer allowed us a 'read-only' version of sp_executesql e.g. sp_executesql_ro, then we could get 
away with only eleven lines of code similar to those below. Also, this would be allowed in a Scalar function, 
as it only outputs data results. By 'read-only' version, I mean that no DML functionality could be processed.

Instead, I had to write over 1500 lines in the form of the  user scalar function that I have submitted.

As for the imagined alternative eleven lines, please see below. Please remember, this code ( when used with sp_executesql ) cannot be used in a user function as it can be used to invoke/action DML commands. 
It ( sp_executesql ) can be used in user stored procedures, but these cannot be called from a select statement.


create or ALTER FUNCTION [dbo].[mthscnvrt09](@strng varchar(200)) RETURNS varchar(50) AS
BEGIN

DECLARE @target1 nVARCHAR(190)

DECLARE @target2B DECIMAL(38,23)  ;

DECLARE @Msg2B NVARCHAR(4000);

DECLARE @return_value varchar(50)

set @target1 = '(958577566+(94447+(cast(966988999 as DECIMAL(38,23))/795)))';

set @Msg2b = N'SELECT @target2B = ' + @target1 ;

execute sp_executesql_ro @Msg2B, N'@target2B DECIMAL(38,23) OUTPUT', @target2B OUTPUT; 

set @return_value = cast( @target2B as varchar(50) )

return (@return_value) 

end

