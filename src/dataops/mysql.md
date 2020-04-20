# MySQL

## Number generator

The purpose is to join or count in special ocasions you can rely on the index, or use a custom order, on old versions without powerful window functions.

You can use maths + join on numbers to create discrete time windows based on created_at, for instance.

```sql
CREATE OR REPLACE VIEW generator_16
AS SELECT 0 n UNION ALL SELECT 1  UNION ALL SELECT 2  UNION ALL 
   SELECT 3   UNION ALL SELECT 4  UNION ALL SELECT 5  UNION ALL
   SELECT 6   UNION ALL SELECT 7  UNION ALL SELECT 8  UNION ALL
   SELECT 9   UNION ALL SELECT 10 UNION ALL SELECT 11 UNION ALL
   SELECT 12  UNION ALL SELECT 13 UNION ALL SELECT 14 UNION ALL 
   SELECT 15;

CREATE OR REPLACE VIEW generator_256
AS SELECT ( ( hi.n << 4 ) | lo.n ) AS n
     FROM generator_16 lo, generator_16 hi;

CREATE OR REPLACE VIEW generator_4k
AS SELECT ( ( hi.n << 8 ) | lo.n ) AS n
     FROM generator_256 lo, generator_16 hi;

CREATE OR REPLACE VIEW generator_64k
AS SELECT ( ( hi.n << 8 ) | lo.n ) AS n
     FROM generator_256 lo, generator_256 hi;

CREATE OR REPLACE VIEW generator_1m
AS SELECT ( ( hi.n << 16 ) | lo.n ) AS n
     FROM generator_64k lo, generator_16 hi;
```

```sql
-- create table for results

drop table if exists numbers ;

create table `numbers` (
  `i` int(11) signed 
  , primary key(`i`)
) ENGINE=myisam DEFAULT CHARSET=latin1;

INSERT INTO numbers(i)
SELECT n FROM generator_64k WHERE n < 64000
```


## Creating database and privileges
    
    mysql -h 127.0.0.1 -uroot -p

```sql
CREATE USER 'ghost'@'%';
GRANT ALL PRIVILEGES ON ghost.* To 'ghost'@'%' IDENTIFIED BY 'ghost123';
FLUSH PRIVILEGES;
```

`CTRL-D` to disconnect