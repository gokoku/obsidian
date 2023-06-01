---
type: note
---

#mysql

---
2023-04-18  11:40

# mysql 8 を入れてrootにゆるゆるパスワードを入れる

```shell
$ mysql_secure_installation

なんか失敗する。
```

とにかく policy を低く低くしたい。
```
$ mysql -u root
mysql> show global variables like '%validate%';

+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| innodb_validate_tablespace_paths     | ON    |
| validate_password.check_user_name    | ON    |
| validate_password.dictionary_file    |       |
| validate_password.length             | 4     |
| validate_password.mixed_case_count   | 0     |
| validate_password.number_count       | 0     |
| validate_password.policy             | LOW   |
| validate_password.special_char_count | 0     |
+--------------------------------------+-------+
8 rows in set (0.00 sec)
```

こうするやり方。
```
mysql> set global validate_password.policy="LOW"
mysql> set global validate_password.special_char_count=0
mysql> set global validate_password.length=4
mysql> set global validate_password.mixed_case_count=0
mysql> set global validate_password.number_count=0
```

これだけやって
```
mysql> alter user "root"@"localhost" identified by "password";
```

弱弱なわかりやすいパスワードにした。

```
$ brew services restart mysql

$ mysql -uroot -p
password

ok!!
```




```sql
select ro.route_id, route_long_name, tr.trip_id, trip_headsign,
       stop_headsign, departure_time,
       monday, tuesday, wednesday, thursday, friday, saturday, sunday
from stop_times as st
left join trips as tr on st.trip_id = tr.trip_id
left join routes as ro on tr.route_id = ro.route_id
left join calendars as cal on tr.service_id = cal.service_id
where (stop_id = "00312-00000000198" or stop_id = "00312-00000000412") and pickup_type = 0 and tuesday = 1
order by ro.route_id asc, departure_time asc;
```

