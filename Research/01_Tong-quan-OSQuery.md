# INTRODUCTION OSQUERY
## Osquery là gì?

OSquery là một framework mã nguồn mở của Facebook cho phép các tổ chức điều tra phát hiện mã độc và các hoạt động động hại trong mạng của mình. OSquery đã có sẵn trên môi trường Mac OS, Linux và Facebook tiếp tục ra mắt một phiên bản dành cho Windows. Khi các kĩ sư Facebook muốn giám sát hàng ngàn máy tính Apple Mac trong mạng nội bộ, họ sử dụng một công cụ an ninh phi truyền thống của riêng mình có tên OSquery. OSquery là một phần mềm đa nền tảng có thể quét từng máy tính trong mạng cụ thể và phân loại chúng. Các truy vấn SQL cho phép nhà phát triển và đội ngũ bảo mật giám sát thời gian thực và nhanh chóng phát hiện hành vi độc hại hay các ứng dụng xấu trong mạng nội bộ. Nói một cách khác, OSquery cho phép các tổ chức biến cơ sở hạ tầng mạng thành một cơ sở dữ liệu, chuyển thông tin hệ điều hành thiết bị thành một định dạng có thể truy vấn thông qua câu lệnh SQL. Tính năng này đặc biệt quan trọng đối với quản trị viên trong xử lý sự cố, chuẩn đoán hệ thống và các vấn đề mức mạng… 

OSquery được ra mắt từ giữa năm 2014 dành cho các bản phân phối Linux như Ubuntu, CentOS và Mac OS X. Đây là một dự án mã nguồn mở  gây được nhiều chú ý trên GitHub. Facebook đã tiếp tục ra mắt phiên bản chính thức dành cho môi trường mạng trên Windows.

Osquery đóng vai trò như một agent trong các giải pháp về bảo mật theo cơ chế HIDS (Host-based intrusion detection system), nó giúp các quản trị viên hệ thống và các các tổ chức điều tra phát hiện mã độc giám sát và phân tích các hoạt động động hại trong môi trường mạng của mình, các truy vấn SQL cho phép giám sát thời gian thực và nhanh chóng phát hiện hành vi độc hại hay các ứng dụng xấu trong mạng nội bộ, tính năng này đặc biệt quan trọng đối với quản trị viên trong xử lý sự cố, chuẩn đoán hệ thống và các vấn đề mức mạng.

## Một số đặc điểm của osquery

Osquery là một framework vô cùng mạnh mẽ với rất nhiều tính năng đặc điểm nổi bật, đóng vai trò như một agent trên một endpoint giúp các nhà phát triển có thể triển khai những giải pháp bảo mật cho hệ thống của mình, sau đây là một số tính năng đặc điểm nổi bật của osquery:

 - Là phần mềm mã nguồn mở hỗ trợ đa nên- tảng và hoàn toàn miễn phí
 - Giám sát hệ thống ở nhiều khía cạnh khác- nhau vô cùng hiệu quả
 - Lưu trữ thông tin về hệ thống trong một- cơ sở dữ liệu quan hệ hiệu năng cao
 - Hỗ trợ API cho phép các nhà phát triển dễ- dàng kết hợp với các nền tảng khác
 - Hỗ trợ lập lịch cho các truy vấn, giúp- thực hiện các truy vấn theo một tru kỳ- xác định
 - Có thể hoạt động như một công cụ giúp các- nhà điều tra phân tích hệ thống một cách- trực quan và hiệu quả
 - Hỗ trợ nhiều phương thức xuất kết quả đầu- ra khác nhau
 - Kết quả đầu ra của các truy vấn dưới định- dạng json giúp dễ dàng thao tác và chuyển- đổi
 - Có thể dễ dàng triển khai kết hợp với các- mô hình khác nhau tạo nên các giải pháp- bảo mật toàn diện

## Osquery có thể thu thập được gì?
OSQuery có thể thu thập được các thông tin như:  
- Running processes.
- User logins.
- Loaded kernel modules.
- Open network connections.
- Browser plugins.
- Hardware events.
- File hashes.
- Sockets.
- Mount.
- Ports.
- Storage volumes.
- Packages.
- Và nhiều thông tin khác.

## Các thành phần của Osquery

Sau khi cài đặt osquery cung cấp kèm theo cho ta đầy đủ các thành phần cho phép ta dễ dàng tương tác và sử dụng, sau đây là các thành phần của osquery:

- `Osqueryi`: Osqueryi là một vỏ (shell) tương tác, nó dùng để thực hiện các truy vấn đặc biệt cũng như các truy vấn nhanh chóng, rất hưu ích khi ta sử dụng nó như một công cụ điều tra, phân tích hệ thống nhằm phát hiện sự cố hoặc điều tra mã độc. Trong chế độ shell này, nó hoàn toàn độc lập, không giao tiếp với trình nền (Osqueryd) và không cần chạy với tư cách quản trị viên (mặc dù một số bảng có thể trả về ít kết quả hơn khi chạy với tư cách không phải quản trị viên).
- `Osqueryd`: Osqueryd là một trình nền (daemon) giám sát máy chủ cho phép ta lên lịch truy vấn và ghi lại các thay đổi trạng thái của hệ điều hành. Trình nền tổng hợp kết quả truy vấn theo thời gian và tạo nhật ký (log), biểu thị trạng thái thay đổi theo từng truy vấn. Trình nền cũng sử dụng API sự kiện hệ điều hành để ghi lại các thay đổi thư mục và tệp được theo dõi, các sự kiện phần cứng, sự kiện mạng và hơn thế nữa.
- `Osqueryctl`: Osqueryctl (osquery controller) là một kịch bản trợ giúp để ta dễ dàng tương tác với trình nền osqueryd như bắt đầu (start), dừng lại (stop) và khởi khộng lại (restart), ngoài ra nó còn hộ trợ ta thử nghiệm triển khai hoặc cấu hình của mình trước khi áp dụng.

## Mô hình triển khai 
Osquery là một framework có khả năng kết hợp với các framework khác để xử lý dữ liệu một cách đa dạng, ví dụ như Server log.

Trong mô hình dưới tất cả những máy chạy OSQuery sẽ gửi dữ liệu thu thập được về một con OSQuery để con này có thể nắm được tình trạng của những con máy Client đó, sau đó gửi những bản tin log thu thập được sang máy Logs Server để xử lý bản tin Log.  
<img src="https://i.imgur.com/J58Ry6j.png">


Osquery có thể cài đặt và làm việc độc lập trên từng máy tính khác nhau hay hoặc kết hợp với stack sản phẩm khác như: `Osquery + Kolide fleet + ELK`, hoặc `Osquery + Kolide fleet + Splunk` hoặc `Osquery + kolide fleet + Graylog` nhằm cung cấp giải pháp giám sát an ninh tổng thể cho hệ thống của bạn. 



