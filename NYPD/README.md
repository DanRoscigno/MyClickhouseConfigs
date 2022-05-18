```
  cat NYPD_create_table| clickhouse-client --password
  zsh ./NYPD_load_command
  echo "select * from NYPD_Complaint where CMPLNT_NUM=400267978" | clickhouse-client --password
```
