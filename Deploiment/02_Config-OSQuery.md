# Config OSQuery
## Allowing osquery to Access the System Log
Mặc định OSQuery không đọc được các Log hệ thống, để OSQuery có thể đọc được thì ta cần phải cấu hình. OSQuery cũng không có sẵn File cấu hình, nên ta sẽ tiến hành tạo File cấu hình này.
Sau đây là danh sách các tùy chọn mà tôi sẽ sử dụng cho tệp cấu hình của tôi, ý nghĩa của chúng và các giá trị mà tôi sẽ đặt cho chúng.

- config_plugin: Nơi đọc cấu hình, thường là một File
- logger_plugin: Chỉ định nơi osquery sẽ ghi kết quả của các truy vấn theo lịch trình.
- logger_path: Đây là đường dẫn đến thư mục nhật ký nơi bạn sẽ tìm thấy các tệp chứa thông tin, cảnh báo, lỗi và kết quả - của các truy vấn theo lịch trình. Mặc định được lưu tại: `/var/log/osquery`
- disable_logging: tùy chọn không ghi log
- log_result_events: By setting this to true, each line in the results logs will represent a state change.
- schedule_splay_percent: Điều này cho osquery biết rằng khi một số lượng lớn các truy vấn được lên lịch trong cùng một khoảng thời gian, để chạy chúng dàn trải để hạn chế bất kỳ tác động hiệu suất nào lên máy chủ. Giá trị mặc định là 10, là 10%.
- pidfile: Nơi để viết id quá trình của deamon osquery. Mặc định là /var/osquery/osquery.pidfile.
- events_expiry: In seconds, how long to hold subscriber results in the osquery backing store. Out of the box, this is set to 3600.
- database_path: OSQuery database path. Mặc định lưu tại: /var/osquery/osquery.db.
- verbose: With logging enabled, this is used to enable or disable verbose informational messages. We’ll set this to - false.
- worker_threads: Số luồng công việc được sử dụng để xử lý các truy vấn, mặc định là 2.
- enable_monitor: enable hoặc Disable lịch trình giám sát, để True.
- disable_events: Used to regulate osquery’s publish/subscribe system. We need this enabled, so the value here will be false.
- disable_audit: Used to disable receiving events from the operating system’s audit subsystem. We need it enabled, so the - value used here will be false.
- audit_allow_config: Allow the audit publisher to change the auditing configuration. Default is true.
- audit_allow_sockets: This allows the audit publisher to install socket-related rules. The value will be true.
- host_identifier: Điều này được sử dụng để xác định máy chủ chạy osquery. Khi kết hợp các kết quả từ nhiều máy chủ, nó giúp dễ dàng xác định máy chủ nào gửi nhật ký hoạt động. Giá trị là hostname chủ hoặc uuid.
- enable_syslog: enable hoặc disable đọc syslog.
- schedule_default_interval: Khi khoảng thời gian truy vấn theo scheduled không được đặt, giá trị này sẽ đực sử dụng.

Chỉnh sửa File cấu hình:
```
vi /etc/osquery/osquery.conf
```
Chỉnh sửa block options thành như sau: 
```
{
  "options": {
    "config_plugin": "filesystem",
    "logger_plugin": "filesystem",
    "logger_path": "/var/log/osquery",
    "disable_logging": "false",
    "log_result_events": "true",
    "schedule_splay_percent": "10",
    "pidfile": "/var/osquery/osquery.pidfile",
    "events_expiry": "3600",
    "database_path": "/var/osquery/osquery.db",
    "verbose": "false",
    "worker_threads": "2",
    "enable_monitor": "true",
    "disable_events": "false",
    "disable_audit": "false",
    "audit_allow_config": "true",
    "host_identifier": "hostname",
    "enable_syslog": "true",
    "audit_allow_sockets": "true",
    "schedule_default_interval": "3600" 
  },
```

tiếp theo ta sẽ config cho block schedule để truy vấn đến `crontab` mỗi 300 giây. Thêm vào File cấu hình như sau
```
  "schedule": {
    "crontab": {
      "query": "SELECT * FROM crontab;",
      "interval": 300
    }
  },
```

Ngoài ra bạn có thể viết bất kỳ số lượng truy vấn nào mà bạn thích. Chỉ cần giữ đúng định dạng.  
Ví dụ:   
Thêm dòng sau vào block schedule.

```
"system_profile": {
      "query": "SELECT * FROM osquery_schedule;"
    }, 
    "system_info": {
      "query": "SELECT hostname, cpu_brand, physical_memory FROM system_info;",
      "interval": 3600
    }
```

File cấu hình sẽ thành như sau: 
```
  "schedule": {
    "crontab": {
      "query": "SELECT * FROM crontab;",
      "interval": 300
    },
    "system_profile": {
      "query": "SELECT * FROM osquery_schedule;"
    }, 
    "system_info": {
      "query": "SELECT hostname, cpu_brand, physical_memory FROM system_info;",
      "interval": 3600
    }
  },
```
<img src="https://i.imgur.com/hcQ0N8v.png">


After the scheduled queries, you can add special queries called decorators, which are queries that prepend data to other scheduled queries. The decorator queries shown here will prepend the UUID of the host running osquery and the username of the user to every scheduled query.  
Append these lines to the file:
```
 "decorators": {
    "load": [
      "SELECT uuid AS host_uuid FROM system_info;",
      "SELECT user AS username FROM logged_in_users ORDER BY time DESC LIMIT 1;"
    ]
  },
```

Cuối cùng, chúng ta có thể trỏ osquery vào danh sách các gói chứa các truy vấn cụ thể. Mỗi cài đặt osquery đi kèm với một bộ gói mặc định nằm trong thư mục `/usr/share/osquery/pack`.

Thêm các dòng này vào tập tin cấu hình:  
```
  "packs": {
     "osquery-monitoring": "/usr/share/osquery/packs/osquery-monitoring.conf",
     "incident-response": "/usr/share/osquery/packs/incident-response.conf",
     "it-compliance": "/usr/share/osquery/packs/it-compliance.conf",
     "vuln-management": "/usr/share/osquery/packs/vuln-management.conf"
  }
}
```

Khi thêm các phần cấu hình bên trên, File cấu hình sẽ có dạng như sau: 
```
{
  "options": {
    "config_plugin": "filesystem",
    "logger_plugin": "filesystem",
    "logger_path": "/var/log/osquery",
    "disable_logging": "false",
    "log_result_events": "true",
    "schedule_splay_percent": "10",
    "pidfile": "/var/osquery/osquery.pidfile",
    "events_expiry": "3600",
    "database_path": "/var/osquery/osquery.db",
    "verbose": "false",
    "worker_threads": "2",
    "enable_monitor": "true",
    "disable_events": "false",
    "disable_audit": "false",
    "audit_allow_config": "true",
    "host_identifier": "hostname",
    "enable_syslog": "true",
    "audit_allow_sockets": "true",
    "schedule_default_interval": "3600"
  },
  "schedule": {
    "crontab": {
      "query": "SELECT * FROM crontab;",
      "interval": 300
    },
    "system_profile": {
      "query": "SELECT * FROM osquery_schedule;"
    },
    "system_info": {
      "query": "SELECT hostname, cpu_brand, physical_memory FROM system_info;",
      "interval": 3600
    }
  },
  "decorators": {
    "load": [
      "SELECT uuid AS host_uuid FROM system_info;",
      "SELECT user AS username FROM logged_in_users ORDER BY time DESC LIMIT 1;"
    ]
  },
  "packs": {
     "osquery-monitoring": "/usr/share/osquery/packs/osquery-monitoring.conf",
     "incident-response": "/usr/share/osquery/packs/incident-response.conf",
     "it-compliance": "/usr/share/osquery/packs/it-compliance.conf",
     "vuln-management": "/usr/share/osquery/packs/vuln-management.conf"
  }
}
```
Restart OSQuery
```
osqueryctl restart
systemctl restart osqueryd
```
Check Config: 
```
osqueryctl config-check
```

# Setting Up the osquery File Integrity Monitoring Pack

Theo dõi chặt chẽ tính toàn vẹn của các tệp trên máy chủ của bạn là một khía cạnh quan trọng trong việc giám sát bảo mật hệ thống. Osquery sẵn sàng cung cấp một giải pháp.  
Đối với phần này, chúng ta sẽ đặt tên tệp là `fim.conf`.  
Create this file and open it in your editor:
```
vi /usr/share/osquery/packs/fim.conf
```

Chúng ta sẽ tạo một pack sẽ theo dõi các tệp trong các thư mục `/home`, `/etc`, và `/tmp` mỗi 5 phút 1 lần. Nó như sau:  

```
{
  "queries": {
    "file_events": {
      "query": "select * from file_events;",
      "removed": false,
      "interval": 300
    }
  },
  "file_paths": {
    "homes": [
      "/root/.ssh/%%",
      "/home/%/.ssh/%%"
    ],
      "etc": [
      "/etc/%%"
    ],
      "home": [
      "/home/%%"
    ],
      "tmp": [
      "/tmp/%%"
    ]
  }
}
```

Thoát và lưu lại.  
Tuy nhiên để nó có thể hoạt động ta cần sửa lại File cấu hình đọc đến pack này, cách thức như sau.
```
vi /etc/osquery/osquery.conf
```
Thêm dòng:  
```
"fim": "/usr/share/osquery/packs/fim.conf",
```
File cấu hình sẽ có dạng như sau:  

<img src="https://i.imgur.com/2d2OptM.png">

Check lại File Config:  
```
osqueryctl config-check
```

Now let’s start using `osqueryi` to query the system.
