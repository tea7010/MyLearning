## Read Pandas df
```python
def read_pandasdf_from_sf(query, conn=conn_sf):
    cs = conn.cursor()
    try:
        cs.execute(query)
        data = cs.fetch_pandas_all()
    finally:
        cs.close()
    return data
```

## Write Pandas df
https://docs.snowflake.com/ja/user-guide/python-connector-api.html#write_pandas

```python
from snowflake.connector.pandas_tools import write_pandas

success, nchunks, nrows, _ = write_pandas(
    conn, 
    test_df,
    database='my_db', schema='schema', table_name='test_table',
    auto_create_table=True,
    overwrite=True,
)
success, nchunks, nrows
```

## When ingesting a timestamp[nz] column
https://stackoverflow.com/questions/66664404/snowflake-write-pandas-is-not-inserting-dates-correctly
```python
def fix_date_cols(df, tz='UTC'):
    cols = df.select_dtypes(include=['datetime64[ns]']).columns
    for col in cols:
        df[col] = df[col].dt.tz_localize(tz)
```