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