# OSQuery Basic

# Using osqueryi to Perform Ad-hoc Security Checks

Có rất nhiều nơi mà osquery có ích. Trong phần này, bạn sẽ thực hiện các kiểm tra bảo mật khác nhau trên hệ thống của mình bằng `osqueryi`

Khởi chạy OSQuery
```
osqueryi --config_path /etc/osquery/osquery.conf --verbose
```

**Lưu ý: Khởi osqueryi và osqueryd, tùy chọn `--verbose` là một cách khởi chạy khuyên dùng hơn là câu lệnh `osqueryi` vì nó cho phép bạn thấy bất kỳ lỗi hoặc cảnh báo nào có thể chỉ ra các vấn đề với osquery.**  
Nào, bây giờ hãy bắt đầu từ những thứ đơn giản nhất, Ví dụ, ai khác ngoài bạn đang đăng nhập vào hệ thống ngay bây giờ?

Sử dụng câu truy vấn: 
```
select * from logged_in_users ;
```
Output: 
```
osquery> select * from logged_in_users ;
+-----------+----------+-------+-----------------------------+------------+------+
| type      | user     | tty   | host                        | time       | pid  |
+-----------+----------+-------+-----------------------------+------------+------+
| boot_time | reboot   | ~     | 3.10.0-1062.12.1.el7.x86_64 | 1583911400 | 0    |
| login     | LOGIN    | tty1  |                             | 1583911407 | 708  |
| runlevel  | runlevel | ~     | 3.10.0-1062.12.1.el7.x86_64 | 1583911409 | 51   |
| user      | root     | pts/0 | 192.168.182.1               | 1583911669 | 1398 |
+-----------+----------+-------+-----------------------------+------------+------+
osquery>
```

Như đã thấy có IP `192.168.182.1` đã đăng nhập thành công và đang sử dụng hệ thống, đó là IP của chính mình, còn nếu bạn thấy IP không phải của mình, thì sẽ phải tiến hành tìm hiểu.  

Truy vấn trước cho bạn biết ai đã đăng nhập ngay bây giờ, nhưng còn lần đăng nhập trước thì sao? Bạn có thể tìm ra bằng cách truy vấn bảng `last`, như thế này:
```
select * from last ;
```
Output:  
```
osquery> select * from last ;
+----------+-------+-------+------+------------+---------------+
| username | tty   | pid   | type | time       | host          |
+----------+-------+-------+------+------------+---------------+
| root     | tty1  | 732   | 7    | 1575022222 |               |
|          | tty1  | 732   | 8    | 1575022290 |               |
| root     | tty1  | 702   | 7    | 1577906559 |               |
|          | tty1  | 702   | 8    | 1577906595 |               |
| root     | tty1  | 707   | 7    | 1577906626 |               |
| root     | pts/0 | 1695  | 7    | 1577907228 | 192.168.182.1 |
|          | pts/0 | 1691  | 8    | 1577919663 |               |
| root     | pts/0 | 35304 | 7    | 1577923192 | 192.168.182.1 |
|          | pts/0 | 35300 | 8    | 1577932045 |               |
| root     | tty1  | 705   | 7    | 1580480857 |               |
| root     | pts/0 | 1394  | 7    | 1580482049 | 192.168.182.1 |
| root     | pts/0 | 1401  | 7    | 1580555652 | 192.168.182.1 |
| root     | tty1  | 712   | 7    | 1580569314 |               |
| root     | pts/1 | 1629  | 7    | 1580569339 | 192.168.182.1 |
|          | pts/0 | 1397  | 8    | 1580570090 |               |
|          | tty1  | 712   | 8    | 1580573810 |               |
| root     | tty1  | 698   | 7    | 1581214397 |               |
| root     | tty1  | 709   | 7    | 1581231853 |               |
| root     | pts/0 | 1389  | 7    | 1581231893 | 192.168.182.1 |
| root     | tty1  | 699   | 7    | 1581682783 |               |
| root     | pts/0 | 1382  | 7    | 1581682857 | 192.168.182.1 |
|          | tty1  | 699   | 8    | 1581702059 |               |
| root     | pts/0 | 1357  | 7    | 1581711312 | 192.168.182.1 |
| root     | tty1  | 702   | 7    | 1583715771 |               |
| root     | pts/0 | 1431  | 7    | 1583719060 | 192.168.182.1 |
|          | pts/0 | 1427  | 8    | 1583719825 |               |
| root     | tty1  | 711   | 7    | 1583745563 |               |
|          | tty1  | 711   | 8    | 1583746986 |               |
| root     | tty1  | 711   | 7    | 1583830748 |               |
| root     | pts/0 | 1410  | 7    | 1583830794 | 192.168.182.1 |
| root     | pts/1 | 1579  | 7    | 1583834445 | 192.168.182.1 |
| root     | pts/2 | 1716  | 7    | 1583836166 | 192.168.182.1 |
|          | pts/0 | 1406  | 8    | 1583837996 |               |
| root     | pts/0 | 1933  | 7    | 1583841912 | 192.168.182.1 |
|          | pts/1 | 1575  | 8    | 1583843230 |               |
|          | pts/2 | 1712  | 8    | 1583848096 |               |
| root     | pts/0 | 1390  | 7    | 1583896522 | 192.168.182.1 |
| root     | tty1  | 706   | 7    | 1583897127 |               |
| root     | pts/0 | 1407  | 7    | 1583897248 | 192.168.182.1 |
| root     | pts/1 | 2507  | 7    | 1583904476 | 192.168.182.1 |
|          | tty1  | 706   | 8    | 1583904670 |               |
| root     | pts/0 | 1398  | 7    | 1583911669 | 192.168.182.1 |
+----------+-------+-------+------+------------+---------------+
osquery>
```
Kiểm tra IPtables
```
select * from iptables ;
```
```
select chain, policy, src_ip, dst_ip from iptables ;
```
Nếu nó không có đầu ra tức là Firewall đang tắt, và điều này không được khuyến khích.  
Những công việc nào được lên lịch trong `crontab`? Bạn có lên lịch cho nó chạy không? Truy vấn này sẽ giúp bạn tìm phần mềm độc hại đã được lên lịch để chạy theo các khoảng thời gian cụ thể:
```
select command, path from crontab ;
```
Output: 
```
osquery> select command, path from crontab ;
+---------------------------------+---------------------+
| command                         | path                |
+---------------------------------+---------------------+
| root run-parts /etc/cron.hourly | /etc/cron.d/0hourly |
+---------------------------------+---------------------+
osquery>
```
Nhìn thấy Output của mình không có gì bất thường.

Are there files on the system that are setuid-enabled? There are quite a few on any Linux server, but which ones are they, and are there any that are not supposed to be on the system? Answers to these question will help you detect backdoored binaries. Run this query periodically and compare its results against older results so that you can keep an eye on any additions. That query is:
```
select * from suid_bin ;
```
Output: 
```
osquery> select * from suid_bin ;
+-------------------------------+----------+-----------+-------------+
| path                          | username | groupname | permissions |
+-------------------------------+----------+-----------+-------------+
| /bin/wall                     | root     | tty       | G           |
| /bin/sg                       | root     | root      | S           |
| /bin/chfn                     | root     | root      | S           |
| /bin/chsh                     | root     | root      | S           |
| /bin/chage                    | root     | root      | S           |
| /bin/gpasswd                  | root     | root      | S           |
| /bin/newgrp                   | root     | root      | S           |
| /bin/mount                    | root     | root      | S           |
| /bin/su                       | root     | root      | S           |
| /bin/umount                   | root     | root      | S           |
| /bin/sudo                     | root     | root      | S           |
| /bin/write                    | root     | tty       | G           |
| /bin/crontab                  | root     | root      | S           |
| /bin/ssh-agent                | root     | nobody    | G           |
| /bin/pkexec                   | root     | root      | S           |
| /bin/passwd                   | root     | root      | S           |
| /bin/sudoedit                 | root     | root      | S           |
| /sbin/unix_chkpwd             | root     | root      | S           |
| /sbin/pam_timestamp_check     | root     | root      | S           |
| /sbin/netreport               | root     | root      | G           |
| /sbin/usernetctl              | root     | root      | S           |
| /sbin/postdrop                | root     | postdrop  | G           |
| /sbin/postqueue               | root     | postdrop  | G           |
| /usr/bin/wall                 | root     | tty       | G           |
| /usr/bin/sg                   | root     | root      | S           |
| /usr/bin/chfn                 | root     | root      | S           |
| /usr/bin/chsh                 | root     | root      | S           |
| /usr/bin/chage                | root     | root      | S           |
| /usr/bin/gpasswd              | root     | root      | S           |
| /usr/bin/newgrp               | root     | root      | S           |
| /usr/bin/mount                | root     | root      | S           |
| /usr/bin/su                   | root     | root      | S           |
| /usr/bin/umount               | root     | root      | S           |
| /usr/bin/sudo                 | root     | root      | S           |
| /usr/bin/write                | root     | tty       | G           |
| /usr/bin/crontab              | root     | root      | S           |
| /usr/bin/ssh-agent            | root     | nobody    | G           |
| /usr/bin/pkexec               | root     | root      | S           |
| /usr/bin/passwd               | root     | root      | S           |
| /usr/bin/sudoedit             | root     | root      | S           |
| /usr/sbin/unix_chkpwd         | root     | root      | S           |
| /usr/sbin/pam_timestamp_check | root     | root      | S           |
| /usr/sbin/netreport           | root     | root      | G           |
| /usr/sbin/usernetctl          | root     | root      | S           |
| /usr/sbin/postdrop            | root     | postdrop  | G           |
| /usr/sbin/postqueue           | root     | postdrop  | G           |
+-------------------------------+----------+-----------+-------------+
osquery>
```
To view a list of loaded kernel modules, run this query:    
```
select name, used_by, status from kernel_modules where status="Live" ;
```
Output: 
```
osquery> select name, used_by, status from kernel_modules where status="Live" ;
+------------------------------+---------------------------------------------------------------------------------+--------+
| name                         | used_by                                                                         | status |
+------------------------------+---------------------------------------------------------------------------------+--------+
| ip6t_rpfilter                | -                                                                               | Live   |
| ip6t_REJECT                  | -                                                                               | Live   |
| nf_reject_ipv6               | ip6t_REJECT                                                                     | Live   |
| ipt_REJECT                   | -                                                                               | Live   |
| nf_reject_ipv4               | ipt_REJECT                                                                      | Live   |
| xt_conntrack                 | -                                                                               | Live   |
| ebtable_nat                  | -                                                                               | Live   |
| ebtable_broute               | -                                                                               | Live   |
| ip6table_nat                 | -                                                                               | Live   |
| nf_conntrack_ipv6            | -                                                                               | Live   |
| nf_defrag_ipv6               | nf_conntrack_ipv6                                                               | Live   |
| nf_nat_ipv6                  | ip6table_nat                                                                    | Live   |
| ip6table_mangle              | -                                                                               | Live   |
| ip6table_security            | -                                                                               | Live   |
| ip6table_raw                 | -                                                                               | Live   |
| iptable_nat                  | -                                                                               | Live   |
| nf_conntrack_ipv4            | -                                                                               | Live   |
| nf_defrag_ipv4               | nf_conntrack_ipv4                                                               | Live   |
| nf_nat_ipv4                  | iptable_nat                                                                     | Live   |
| nf_nat                       | nf_nat_ipv6,nf_nat_ipv4                                                         | Live   |
| iptable_mangle               | -                                                                               | Live   |
| iptable_security             | -                                                                               | Live   |
| iptable_raw                  | -                                                                               | Live   |
| nf_conntrack                 | xt_conntrack,nf_conntrack_ipv6,nf_nat_ipv6,nf_conntrack_ipv4,nf_nat_ipv4,nf_nat | Live   |
| ebtable_filter               | -                                                                               | Live   |
| ebtables                     | ebtable_nat,ebtable_broute,ebtable_filter                                       | Live   |
| ip6table_filter              | -                                                                               | Live   |
| ip6_tables                   | ip6table_nat,ip6table_mangle,ip6table_security,ip6table_raw,ip6table_filter     | Live   |
| iptable_filter               | -                                                                               | Live   |
| ip_tables                    | iptable_nat,iptable_mangle,iptable_security,iptable_raw,iptable_filter          | Live   |
| bridge                       | ebtable_broute                                                                  | Live   |
| stp                          | bridge                                                                          | Live   |
| llc                          | bridge,stp                                                                      | Live   |
| ip_set                       | -                                                                               | Live   |
| nfnetlink                    | ip_set                                                                          | Live   |
| snd_seq_midi                 | -                                                                               | Live   |
| snd_seq_midi_event           | snd_seq_midi                                                                    | Live   |
| iosf_mbi                     | -                                                                               | Live   |
| crc32_pclmul                 | -                                                                               | Live   |
| ghash_clmulni_intel          | -                                                                               | Live   |
| snd_ens1371                  | -                                                                               | Live   |
| ppdev                        | -                                                                               | Live   |
| snd_rawmidi                  | snd_seq_midi,snd_ens1371                                                        | Live   |
| snd_ac97_codec               | snd_ens1371                                                                     | Live   |
| btusb                        | -                                                                               | Live   |
| aesni_intel                  | -                                                                               | Live   |
| ac97_bus                     | snd_ac97_codec                                                                  | Live   |
| btrtl                        | btusb                                                                           | Live   |
| snd_seq                      | snd_seq_midi,snd_seq_midi_event                                                 | Live   |
| btbcm                        | btusb                                                                           | Live   |
| btintel                      | btusb                                                                           | Live   |
| lrw                          | aesni_intel                                                                     | Live   |
| snd_seq_device               | snd_seq_midi,snd_rawmidi,snd_seq                                                | Live   |
| gf128mul                     | lrw                                                                             | Live   |
| bluetooth                    | btusb,btrtl,btbcm,btintel                                                       | Live   |
| vmw_balloon                  | -                                                                               | Live   |
| glue_helper                  | aesni_intel                                                                     | Live   |
| ablk_helper                  | aesni_intel                                                                     | Live   |
| cryptd                       | ghash_clmulni_intel,aesni_intel,ablk_helper                                     | Live   |
| snd_pcm                      | snd_ens1371,snd_ac97_codec                                                      | Live   |
| joydev                       | -                                                                               | Live   |
| pcspkr                       | -                                                                               | Live   |
| snd_timer                    | snd_seq,snd_pcm                                                                 | Live   |
| sg                           | -                                                                               | Live   |
| snd                          | snd_ens1371,snd_rawmidi,snd_ac97_codec,snd_seq,snd_seq_device,snd_pcm,snd_timer | Live   |
| rfkill                       | bluetooth                                                                       | Live   |
| soundcore                    | snd                                                                             | Live   |
| vmw_vmci                     | -                                                                               | Live   |
| i2c_piix4                    | -                                                                               | Live   |
| parport_pc                   | -                                                                               | Live   |
| parport                      | ppdev,parport_pc                                                                | Live   |
| pcc_cpufreq                  | -                                                                               | Live   |
| xfs                          | -                                                                               | Live   |
| libcrc32c                    | nf_nat,nf_conntrack,xfs                                                         | Live   |
| sr_mod                       | -                                                                               | Live   |
| cdrom                        | sr_mod                                                                          | Live   |
| ata_generic                  | -                                                                               | Live   |
| pata_acpi                    | -                                                                               | Live   |
| vmwgfx                       | -                                                                               | Live   |
| sd_mod                       | -                                                                               | Live   |
| crc_t10dif                   | sd_mod                                                                          | Live   |
| crct10dif_generic            | -                                                                               | Live   |
| drm_kms_helper               | vmwgfx                                                                          | Live   |
| syscopyarea                  | drm_kms_helper                                                                  | Live   |
| sysfillrect                  | drm_kms_helper                                                                  | Live   |
| sysimgblt                    | drm_kms_helper                                                                  | Live   |
| fb_sys_fops                  | drm_kms_helper                                                                  | Live   |
| ttm                          | vmwgfx                                                                          | Live   |
| crct10dif_pclmul             | -                                                                               | Live   |
| crct10dif_common             | crc_t10dif,crct10dif_generic,crct10dif_pclmul                                   | Live   |
| crc32c_intel                 | -                                                                               | Live   |
| drm                          | vmwgfx,drm_kms_helper,ttm                                                       | Live   |
| ata_piix                     | -                                                                               | Live   |
| nfit                         | -                                                                               | Live   |
| libata                       | ata_generic,pata_acpi,ata_piix                                                  | Live   |
| mptspi                       | -                                                                               | Live   |
| scsi_transport_spi           | mptspi                                                                          | Live   |
| serio_raw                    | -                                                                               | Live   |
| e1000                        | -                                                                               | Live   |
| libnvdimm                    | nfit                                                                            | Live   |
| mptscsih                     | mptspi                                                                          | Live   |
| mptbase                      | mptspi,mptscsih                                                                 | Live   |
| drm_panel_orientation_quirks | drm                                                                             | Live   |
| dm_mirror                    | -                                                                               | Live   |
| dm_region_hash               | dm_mirror                                                                       | Live   |
| dm_log                       | dm_mirror,dm_region_hash                                                        | Live   |
| dm_mod                       | dm_mirror,dm_log                                                                | Live   |
+------------------------------+---------------------------------------------------------------------------------+--------+
osquery>
```
Đây là một truy vấn khác mà bạn cũng sẽ muốn chạy định kỳ và so sánh kết quả đầu ra của nó với các kết quả cũ hơn để xem có gì thay đổi không.

Tuy nhiên, một phương pháp khác sẽ giúp bạn tìm thấy các backdoors trên máy chủ là chạy một truy vấn liệt kê tất cả các ports.
```
select * from listening_ports ;
```
Output: 
```
osquery> select * from listening_ports ;
+------+-------+----------+--------+-----------+----+--------+------------------------------+---------------+
| pid  | port  | protocol | family | address   | fd | socket | path                         | net_namespace |
+------+-------+----------+--------+-----------+----+--------+------------------------------+---------------+
| 1238 | 25    | 6        | 2      | 127.0.0.1 | 13 | 21276  |                              | 4026531956    |
| 1037 | 22    | 6        | 2      | 0.0.0.0   | 3  | 20546  |                              | 4026531956    |
| 1238 | 25    | 6        | 10     | ::1       | 14 | 21277  |                              | 4026531956    |
| 1090 | 33060 | 6        | 10     | ::        | 33 | 21555  |                              | 4026531956    |
| 1090 | 3306  | 6        | 10     | ::        | 30 | 21550  |                              | 4026531956    |
| 1037 | 22    | 6        | 10     | ::        | 4  | 20573  |                              | 4026531956    |
| 714  | 58    | 255      | 10     | ::        | 18 | 27027  |                              | 4026531956    |
| 1    | 0     | 0        | 1      |           | 24 | 0      | /run/systemd/notify          | 4026531956    |
| 692  | 0     | 0        | 1      |           | 3  | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 1    | 0     | 0        | 1      |           | 25 | 0      | /run/systemd/cgroups-agent   | 4026531956    |
| 1090 | 0     | 0        | 1      |           | 32 | 0      | /var/lib/mysql/mysql.sock    | 4026531956    |
| 497  | 0     | 0        | 1      |           | 3  | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 4  | 0      | /run/systemd/journal/socket  | 4026531956    |
| 1242 | 0     | 0        | 1      |           | 6  | 0      | public/qmgr                  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 5  | 0      | /dev/log                     | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 47 | 0      | public/flush                 | 4026531956    |
| 1090 | 0     | 0        | 1      |           | 34 | 0      | /var/run/mysqld/mysqlx.sock  | 4026531956    |
| 1    | 0     | 0        | 1      |           | 12 | 0      | /run/systemd/private         | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 41 | 0      | private/trace                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 44 | 0      | private/verify               | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 50 | 0      | private/proxymap             | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 53 | 0      | private/proxywrite           | 4026531956    |
| 1714 | 0     | 0        | 1      |           | 20 | 0      | /var/osquery/osquery.em      | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 56 | 0      | private/smtp                 | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 59 | 0      | private/relay                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 65 | 0      | private/error                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 68 | 0      | private/retry                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 71 | 0      | private/discard              | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 74 | 0      | private/local                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 77 | 0      | private/virtual              | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 80 | 0      | private/lmtp                 | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 83 | 0      | private/anvil                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 86 | 0      | private/scache               | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 29 | 0      | private/tlsmgr               | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 32 | 0      | private/rewrite              | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 35 | 0      | private/bounce               | 4026531956    |
| 1746 | 0     | 0        | 1      |           | 8  | 0      | /root/.osquery/shell.em      | 4026531956    |
| 2111 | 0     | 0        | 1      |           | 8  | 0      | /root/.osquery/shell.em11184 | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 62 | 0      | public/showq                 | 4026531956    |
| 1    | 0     | 0        | 1      |           | 23 | 0      | /run/lvm/lvmpolld.socket     | 4026531956    |
| 528  | 0     | 0        | 1      |           | 4  | 0      | /run/udev/control            | 4026531956    |
| 518  | 0     | 0        | 1      |           | 3  | 0      | /run/lvm/lvmetad.socket      | 4026531956    |
| 2103 | 0     | 0        | 1      |           | 6  | 0      | public/pickup                | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 22 | 0      | public/cleanup               | 4026531956    |
| 1238 | 0     | 0        | 1      |           | 38 | 0      | private/defer                | 4026531956    |
| 1    | 0     | 0        | 1      |           | 32 | 0      | /run/systemd/shutdownd       | 4026531956    |
| 497  | 0     | 0        | 1      |           | 19 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 17 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 17 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 24 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 16 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 20 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 10 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 18 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 16 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 14 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 21 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 14 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 11 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 497  | 0     | 0        | 1      |           | 15 | 0      | /run/systemd/journal/stdout  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 12 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
| 692  | 0     | 0        | 1      |           | 19 | 0      | /run/dbus/system_bus_socket  | 4026531956    |
+------+-------+----------+--------+-----------+----+--------+------------------------------+---------------+
osquery>
```
Trên máy chủ của bạn, nếu đầu ra chỉ bao gồm các port mà bạn biết máy chủ nên lắng nghe, thì không có gì phải lo lắng. Nhưng nếu có các port khác mở, bạn sẽ muốn điều tra xem các port đó là gì.
```
select * from listening_ports ;
```
Để xem hoạt động của tệp trên máy chủ, chạy truy vấn sau:  
```
select target_path, action, uid from file_events ;
```

Bạn có thể liệt kê các bảng với:
```
.tables
```

# Running osqueryd 

Osqueryd, daemon, cho phép osquery chạy các truy vấn theo các khoảng thời gian đã đặt trước. Các truy vấn đó bao gồm các truy vấn bạn đã định như các bước bên trên, và bao gồm cả các truy vấn trong các pack chúng ta đã chỉ định trong bước đó và trong gói FIM chúng ta đã định cấu hình. Nếu bạn chưa nghiên cứu chúng, hãy xem xét nội dung của `/usr/share/osquery/pack`.

Các kết quả được tạo bởi osqueryd được ghi vào một tệp có tên `osqueryd.results.log` trong thư mục `/var/log/osquery`.






















