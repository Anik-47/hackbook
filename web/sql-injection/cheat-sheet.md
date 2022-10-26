# cheat-sheet

## String Concatenation <a href="#string-concatenation" id="string-concatenation"></a>

* <mark style="color:red;">Oracle:</mark> _<mark style="color:blue;">`'foo'||'bar'`</mark>_&#x20;
* <mark style="color:red;">Microsoft:</mark> _<mark style="color:blue;">`'foo'+'bar'`</mark>_  &#x20;
* <mark style="color:red;">PostgreSQL:</mark> _<mark style="color:blue;">`'foo'||'bar'`</mark>_
* <mark style="color:red;">MySQL:</mark> _<mark style="color:blue;">`'foo' 'bar'`</mark>_  <mark style="color:blue;"></mark><mark style="color:blue;"></mark>  <mark style="color:blue;"></mark>_<mark style="color:blue;">`CONCAT('foo','bar')`</mark>_

## Database Version

* <mark style="color:red;">Oracle:</mark> _<mark style="color:blue;">`SELECT banner FROM v$version`</mark>_ & _<mark style="color:blue;">`SELECT version FROM v$instance`</mark>_
* <mark style="color:red;">Microsoft:</mark> _<mark style="color:blue;">`SELECT @@version`</mark>_
* <mark style="color:red;">PostgreSQL:</mark> _<mark style="color:blue;">`SELECT version()`</mark>_
* <mark style="color:red;">MySQL:</mark> _<mark style="color:blue;">`SELECT @@version`</mark>_

## Database Contents

* <mark style="color:red;">Oracle:</mark> _<mark style="color:blue;">`SELECT table_name FROM all_tables`</mark>_

&#x20;                 _<mark style="color:blue;">`SELECT FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'`</mark>_

* <mark style="color:red;">Microsoft:</mark> _<mark style="color:blue;">`SELECT table_name FROM information_schema.tables`</mark>_ &#x20;

&#x20;                       _<mark style="color:blue;">`SELECT column_name FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'`</mark>_&#x20;

* <mark style="color:red;">PostgreSQL:</mark> _<mark style="color:blue;">`SELECT table_name FROM information_schema.tables`</mark>_

&#x20;                        _<mark style="color:blue;">`SELECT column_name FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'`</mark>_

* <mark style="color:red;">MySQL:</mark> _<mark style="color:blue;">`SELECT table_name FROM information_schema.tables`</mark>_

&#x20;                       _<mark style="color:blue;">`SELECT column_name FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'`</mark>_
