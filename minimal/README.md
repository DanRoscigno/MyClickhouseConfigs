### query the TSV file

- Copy the TSV file to the `user_dir` as specified in `config.xml`
or your override, and set the file ownership to the user that runs
`clickhouse-server`
- Run this command in `clickhouse-client`
    ```
    select ADDR_PCT_CD, count() from
    file('NYPD_Complaint_Data_Current__Year_To_Date_.tsv', 'TSVWithNames')
    group by ADDR_PCT_CD
    ```

### reset and check

```
echo "drop table minimal" | clickhouse-client --password
cat minimal_create_table| clickhouse-client --password
zsh ./minimal_load
echo "select * from minimal" | clickhouse-client --password
```
