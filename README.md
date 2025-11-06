# bai_so_3_web
Bài tập 3   : môn Phát triển ứng dụng trên nền web
--------------------------------------------------
Yêu cầu     : LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
 - enable wsl: cài đặt docker desktop
 - enable wsl: cài đặt ubuntu
 - sử dụng Hyper-V: cài đặt ubuntu
 - sử dụng VMware : cài đặt ubuntu
 - sử dụng Virtual Box: cài đặt ubuntu
2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay)
3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
4. Lập trình web frontend+backend:
 SV chọn 1 trong các web sau:
 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
 - Có tính năng liệt kê các nhóm sản phẩm
 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
 4.2 Web IOT: Giám sát dữ liệu IOT.
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - hiển thị giá trị mới nhất của các thông số đang giám sát, khi click vào thì hiển thị đồ thị lịch sử quá trình thay đổi (gọi grafana iframe để hiển thị)
 - backend: Sử dụng nodered để đọc dữ liệu từ các cảm biến (có thể dùng api online để lấy dữ liệu theo giời gian thực), 
   nodered sẽ lưu dữ liệu mới nhất (dạng update) vào cơ sở dữ liệu mariadb (sử dụng phpmyadmin để tạp table và quản trị lần đầu)
   nodered sẽ lưu dữ liệu (insert) vào influxdb để lưu giá trị lịch sử, để cho grafana dùng để hiển thị biểu đồ.
5. Nginx làm web-server
 - Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)

Yêu cầu sinh viên lưu code trên github
có file readme.md có hình ảnh + text: ghi lại nhật ký quá trình làm bài.

# 1 cài đặt môi trường 
dùng wsl 2
<img width="1074" height="541" alt="image" src="https://github.com/user-attachments/assets/c5dfc114-9d2c-43b3-8748-7788ab00a7a1" />

Sau khi cài xong, gõ:

wsl --set-default-version 2

wsl --install -d Ubuntu

<img width="1058" height="552" alt="image" src="https://github.com/user-attachments/assets/7ff8e06b-ad69-4e25-8413-e1d447cba7c1" />


Mở Ubuntu → đặt username và password.

Cập nhật:

sudo apt update && sudo apt upgrade -y

<img width="486" height="286" alt="image" src="https://github.com/user-attachments/assets/3251e795-a1a6-4c47-b6e3-e8c289f32e1d" />

cài ubuntu thành công 

<img width="1073" height="509" alt="image" src="https://github.com/user-attachments/assets/f2210d46-3fe9-4d5b-9639-290720a52d1a" />

# 2 cài đặt docker desktop 

<img width="1827" height="959" alt="image" src="https://github.com/user-attachments/assets/b22f1d14-66c0-4c41-80b3-8778fed645a1" />

docker chạy thành công 

<img width="1275" height="703" alt="image" src="https://github.com/user-attachments/assets/8a3887dc-f53b-464c-bbbe-4dc095356703" />

# 3 chay file docker-yml.com

<img width="648" height="240" alt="image" src="https://github.com/user-attachments/assets/73f7c098-9022-4506-8dac-6e0dfbb8812b" />

dán file vào 

<img width="1919" height="873" alt="image" src="https://github.com/user-attachments/assets/a54bf1f3-9a39-4846-94c5-09bb3489a5ea" />

ctrl + o +enter để lưu 

docker compose -d

<img width="1714" height="395" alt="image" src="https://github.com/user-attachments/assets/0289dc98-6827-4da6-8bc9-03478a81d1c4" />

file chạy thành công 

<img width="1710" height="402" alt="image" src="https://github.com/user-attachments/assets/bc9115f0-f2fa-4529-9021-bb57103d19c6" />

ấn docker ps
<img width="1741" height="248" alt="image" src="https://github.com/user-attachments/assets/f1b7dfd2-1bd6-4c2f-992c-4a15169dc10e" />




# 5 nginx làm web sever 

sử dụng câu lệnh sudo nano /etc/hosts
<img width="1918" height="986" alt="image" src="https://github.com/user-attachments/assets/e94aa01a-37d3-4bb7-b20c-60ca1a775592" />

đổi câu lệnh notepad C:\Windows\System32\drivers\etc\hosts

<img width="922" height="828" alt="image" src="https://github.com/user-attachments/assets/e6c7c993-9b48-4002-9ba7-6083074e340d" />

kết quả 

<img width="623" height="53" alt="image" src="https://github.com/user-attachments/assets/aa8dfd9e-c7ea-4b8c-a6b0-8f4587bfaeec" />

<img width="425" height="52" alt="image" src="https://github.com/user-attachments/assets/5db82bff-f041-426c-8f5d-e231e51e0410" />

# hệ thống iot 

cấu hình phpadmin 

<img width="1870" height="997" alt="image" src="https://github.com/user-attachments/assets/764cabc4-03b0-4090-8a97-32f06f43435f" />

cấu hình nodered

<img width="1464" height="827" alt="image" src="https://github.com/user-attachments/assets/fc5df06c-e6e7-4dd9-a23d-a3d444548338" />

kết quả 

<img width="446" height="411" alt="image" src="https://github.com/user-attachments/assets/559a68dd-219f-48ec-a06d-a24e355ba356" />

kết quả chạy html 
gọi được API dữ liệu 
<img width="1836" height="912" alt="image" src="https://github.com/user-attachments/assets/4fb85124-313f-4614-8522-a00fb1780461" />




