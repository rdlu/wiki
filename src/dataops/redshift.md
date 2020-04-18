# Redshift

## Grant read only access to a group on a schema

```sql
-- Create Read-Only Group     
CREATE GROUP ro_group;

-- Create User
CREATE USER ro_user WITH password PASSWORD;

-- Add User to Read-Only Group
ALTER GROUP ro_group ADD USER ro_user;

-- Grant Usage permission to Read-Only Group to specific Schema
GRANT USAGE ON SCHEMA "ro_schema" TO GROUP ro_group;

-- Grant Select permission to Read-Only Group to specific Schema
GRANT SELECT ON ALL TABLES IN SCHEMA "ro_schema" TO GROUP ro_group;

-- Alter Default Privileges to maintain the permissions on new tables
ALTER DEFAULT PRIVILEGES IN SCHEMA "ro_schema" GRANT SELECT ON TABLES TO GROUP ro_group;

-- Revoke CREATE privileges from group
REVOKE CREATE ON SCHEMA "ro_schema" FROM GROUP ro_group;
```

## Testing sorting keys

```sql
create table datasci.test_table_compound
compound sortkey (created_at, id)
as select * from public.test_table;

create table datasci.test_table_interleaved
interleaved sortkey (created_at, id)
as select * from public.test_table;
```

This copy for me over a million rows tables works pretty fast (< 1 minute), so it's worthy to copy some optimized tables.