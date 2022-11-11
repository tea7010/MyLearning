## Install
https://www.postgresql.org/download/linux/ubuntu/

## Start
https://rowingfan.hatenablog.jp/entry/2020/11/07/135110

### Confirm version
```
psql --version
```

### Start server
```
sudo service postgresql start
```

### Stop
```
sudo service postgresql stop
```

### Login as postgres user
```
sudo -u postgres psql
```

### Logout
```sql
\q
```

### List of database
```
psql -l
```

### Create user
```sql
postgres=# create role [Ubuntuのユーザ名] with login createdb;
```

## How to connect Windoes PostgreSQL from WSL2
https://stackoverflow.com/questions/56824788/how-to-connect-to-windows-postgres-database-from-wsl

# Python

## Python connector usage
https://www.postgresqltutorial.com/postgresql-python/connect/

## Pandas read/write
https://towardsdatascience.com/upload-your-pandas-dataframe-to-your-database-10x-faster-eb6dc6609ddf`

```python
import pandas as pd
from sqlalchemy import create_engine
import psycopg2

conn_psql = {
    'user': 'postgres',
    'password': '',
    'host': 'winhost',
    'port': '5432',
    'database': 'postgres'
}

def read_pandasdf_from_psql(query, conn=conn_psql):
    conn = psycopg2.connect(**conn_psql)
    return pd.read_sql(sql=query, con=conn)

def write_pandasdf_to_psql(df, table_nam, if_exists='replace', conn=conn_psql):
    engine = create_engine('postgresql://{user}:{password}@{host}:{port}/{database}'.format(**conn_psql))
    df.to_sql(table_nam, con=engine, if_exists=if_exists, index=False)
```

## List tables

```sql
SELECT * FROM information_schema.tables where table_schema='public'
```
