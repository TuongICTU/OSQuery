# Install OSQuery 

Update hệ thống và cài đặt Wget 

```
yum update -y && yum install -y wget
```
Cài đặt gói yum-utils
```
yum install yum-utils
```
Set hostname cho máy
```
hostnamectl set-hostname OSQueryClient
bash
```

Thực hiện lần lượt các câu lệnh sau: 
```
curl -L https://pkg.osquery.io/rpm/GPG | sudo tee /etc/pki/rpm-gpg/RPM-GPG-KEY-osquery  
sudo yum-config-manager --add-repo https://pkg.osquery.io/rpm/osquery-s3-rpm.repo  
sudo yum-config-manager --enable osquery-s3-rpm  
sudo yum install osquery
cp /usr/share/osquery/osquery.example.conf /etc/osquery/osquery.conf
```

Kiểm tra phiên bản osquery vừa cài đặt bằng lệnh osqueryd -version
```
[root@osqueryclient ~]# osqueryd -version
osqueryd version 4.1.2
```

File cấu hình của OSQuery nằm tại `/etc/osquery/osquery.conf`

Nội dung có cấu trúc như sau: 
```
{
    "options": {
        // Các khai báo 
    },

    "schedule": {
        // Các khai báo
    },

     "decorators": {
         //Các khai báo

     },

     "packs": {
         //Các khai báo
     },

     // Và các khai báo khác.

}
```

Trong đó: 

- "options": Khai các tùy chọn về lưu trữ dữ liệu, tùy chọn về file log.
- "schedule": Khai báo để lập lịch định kỳ chạy các câu truy vấn mà ta chỉ định.
- "packs": Là đường dẫn chứa các khai báo các cấu hình chuyên biệt theo nhóm được cấu hình sẵn. 

## Chỉnh sửa file cấu hình 
Sửa dòng
```
//"logger_path": "/var/log/osquery",
```
Thành dòng
```
"logger_path": "/var/log/osquery",
```

## Thêm cấu hình

 Dưới dòng `"schedule": { `thêm đoạn cấu hình dưới
```
  // for example, get CPU Time per 60 seconds
    "cpu_time": {
      "query": "SELECT * FROM cpu_time;",
      "interval": 60
    },

    // for example, get settings of resolv.conf per an hour
    "dns_resolvers": {
      "query": "SELECT * FROM dns_resolvers;",
      "interval": 120
    },
```

Khởi động osquery
```
systemctl start osqueryd
systemctl enable osqueryd
```
Kiểm tra:  
```
systemctl status osqueryd
```

Mở cửa sổ thao tác với `osquery` bằng lệnh `osqueryi`
```
[root@osqueryclient osquery]# osqueryi
Using a virtual database. Need help, type '.help'
osquery>
```
Dùng câu lệnh SQL để truy vấn về thông tin hệ điều hành: 
```
osquery> select * from os_version; 
+--------------+--------------------------------------+-------+-------+-------+-------+----------+---------------+----------+
| name         | version                              | major | minor | patch | build | platform | platform_like | codename |
+--------------+--------------------------------------+-------+-------+-------+-------+----------+---------------+----------+
| CentOS Linux | CentOS Linux release 7.7.1908 (Core) | 7     | 7     | 1908  |       | rhel     | rhel          |          |
+--------------+--------------------------------------+-------+-------+-------+-------+----------+---------------+----------+

```
Uptime
```
osquery> select * from uptime;
+------+-------+---------+---------+---------------+
| days | hours | minutes | seconds | total_seconds |
+------+-------+---------+---------+---------------+
| 0    | 1     | 17      | 36      | 4656          |
+------+-------+---------+---------+---------------+
```

Các User trong hệ thống: 
```
osquery> select * from users;
+-----+-----+------------+------------+-----------------+----------------------------+--------------------+----------------+------+
| uid | gid | uid_signed | gid_signed | username        | description                | directory          | shell          | uuid |
+-----+-----+------------+------------+-----------------+----------------------------+--------------------+----------------+------+
| 0   | 0   | 0          | 0          | root            | root                       | /root              | /bin/bash      |      |
| 1   | 1   | 1          | 1          | bin             | bin                        | /bin               | /sbin/nologin  |      |
| 2   | 2   | 2          | 2          | daemon          | daemon                     | /sbin              | /sbin/nologin  |      |
| 3   | 4   | 3          | 4          | adm             | adm                        | /var/adm           | /sbin/nologin  |      |
| 4   | 7   | 4          | 7          | lp              | lp                         | /var/spool/lpd     | /sbin/nologin  |      |
| 5   | 0   | 5          | 0          | sync            | sync                       | /sbin              | /bin/sync      |      |
| 6   | 0   | 6          | 0          | shutdown        | shutdown                   | /sbin              | /sbin/shutdown |      |
| 7   | 0   | 7          | 0          | halt            | halt                       | /sbin              | /sbin/halt     |      |
| 8   | 12  | 8          | 12         | mail            | mail                       | /var/spool/mail    | /sbin/nologin  |      |
| 11  | 0   | 11         | 0          | operator        | operator                   | /root              | /sbin/nologin  |      |
| 12  | 100 | 12         | 100        | games           | games                      | /usr/games         | /sbin/nologin  |      |
| 14  | 50  | 14         | 50         | ftp             | FTP User                   | /var/ftp           | /sbin/nologin  |      |
| 99  | 99  | 99         | 99         | nobody          | Nobody                     | /                  | /sbin/nologin  |      |
| 192 | 192 | 192        | 192        | systemd-network | systemd Network Management | /                  | /sbin/nologin  |      |
| 81  | 81  | 81         | 81         | dbus            | System message bus         | /                  | /sbin/nologin  |      |
| 999 | 998 | 999        | 998        | polkitd         | User for polkitd           | /                  | /sbin/nologin  |      |
| 74  | 74  | 74         | 74         | sshd            | Privilege-separated SSH    | /var/empty/sshd    | /sbin/nologin  |      |
| 89  | 89  | 89         | 89         | postfix         |                            | /var/spool/postfix | /sbin/nologin  |      |
| 27  | 27  | 27         | 27         | mysql           | MySQL Server               | /var/lib/mysql     | /bin/false     |      |
+-----+-----+------------+------------+-----------------+----------------------------+--------------------+----------------+------+
```


Ta có thể giới hạn kết quả bằng tùy chọn limit. Giả sử chỉ cần hiện 05 user đầu tiên ta sẽ cùng lệnh select * from users limit 5;
```
osquery> select * from users limit 5;
+-----+-----+------------+------------+----------+-------------+----------------+---------------+------+
| uid | gid | uid_signed | gid_signed | username | description | directory      | shell         | uuid |
+-----+-----+------------+------------+----------+-------------+----------------+---------------+------+
| 0   | 0   | 0          | 0          | root     | root        | /root          | /bin/bash     |      |
| 1   | 1   | 1          | 1          | bin      | bin         | /bin           | /sbin/nologin |      |
| 2   | 2   | 2          | 2          | daemon   | daemon      | /sbin          | /sbin/nologin |      |
| 3   | 4   | 3          | 4          | adm      | adm         | /var/adm       | /sbin/nologin |      |
| 4   | 7   | 4          | 7          | lp       | lp          | /var/spool/lpd | /sbin/nologin |      |
+-----+-----+------------+------------+----------+-------------+----------------+---------------+------+
```

Ta có thể sử dụng tùy chọn count(*) để hiển thị tổng user (Giống như câu truy vấn trong SQL).
```
select count(*) from users;
```
```
osquery> select count(*) from users;
+----------+
| count(*) |
+----------+
| 19       |
+----------+
```






































Về các bảng truy vấn trên đây đều được liệt kê tại https://osquery.io/schema/4.1.2











